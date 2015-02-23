---
layout: post
title: Powershell Remoting / Exchange 2010
date: 2014-01-14
categories:
  - SysAdmin
tags:
  - Exchange
  - PowerShell
---

I was tasked with updating our user creation / tear down tool to set an out of
office message for users when their account is disabled following their
termination.

Initially, I thought this would be as simple as adding a function to run
`Set-MailboxConfigurationAutoReply` but every time the app tried to set a new
out of office message I received the error: 'Operation is not valid due to the
current state of the object.' <!-- more -->

Some google-fu led me to the fact that the way the Exchange commands were being
loaded in the app is not supported in Exchange 2010. This method bypasses RBAC
and in general, many commands just fail to work when a remote connection is
established by force loading the snap-in and dot-sourcing the Remote Exchange
script in a standard PoSH session:

``` powershell Loading snap-in via remoting
$Snapin = "Microsoft.Exchange.Management.PowerShell.E2010"

if(Get-PSSnapin -Name $Snapin -ErrorAction SilentlyContinue) {}
elseif(Get-PSSnapin -Name $Snapin -Registered -ErrorAction SilentlyContinue){  
  Add-PSSnapin $Snapin  
  . $env:ExchangeInstallPath\bin\RemoteExchange.ps1  
  Connect-ExchangeServer -auto  
}  
else{$SB.Text = "PSSnapin $Snapin not found"}
```

With that being said, I'm surprised the Exchange commands in the app have
functioned up until this point without using remoting.

I replaced the above code with the following:

``` powershell Loading snap-in without remoting
$s = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri http://<server>/PowerShell/ -Authentication Kerberos
Import-PSSession $s
```

Now, everything works as it should including the
`Set-MailboxConfigurationAutoReply` command.

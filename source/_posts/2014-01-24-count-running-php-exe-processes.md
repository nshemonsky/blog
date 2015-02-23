---
title: Count running PHP processes with PowerShell
author: nshemonsky
layout: post
categories:
  - SysAdmin
tags:
  - PowerShell
---
We have a server that seems to end up going haywire every Friday afternoon,
spawning countless PHP processes. Last week it spiked at over 500 but we didn't
know when it began or ended specifically.
<!-- more -->

I threw together a quick PowerShell script to handle capturing the number of
running processes on a specified interval. This could easily be modified to
ask for any process and server as well making it extremely useful.

``` powershell CountPHP_Processes.ps1
Function CountPHP
{
<
    .SYNOPSIS
    Returns count of running PHP processes on &lt;Server&gt;

    .PARAMETER Path
    Specifies the folder path.

    .PARAMETER Interval
    Specifies the interval in seconds between running
    Get-Process on &lt;Server&gt;.

    .PARAMETER EndTime
    Specifies when the script should stop running in the
    format HH:mm

    .EXAMPLE
    CountPHP -Path H:\ -Interval 60 -EndTime 14:00

    Outputs count of php.exe on &lt;Server&gt; every 60 seconds
    from run time until 2pm.
>

    [CmdletBinding()] Param(
        [Parameter(Mandatory=$True)]
        [ValidateScript({Test-Path -Path $_ })]
        [String] $Path,

        [Parameter(Mandatory=$True)]
        [Int32] $Interval,

        [Parameter(Mandatory=$True)]
        [String] $EndTime
    )

    Do {
        #Get the current time
        $Time = (Get-Date)

        [String] $Filename = "PHP_Count_" + "$($Time.Month)" + "_" + "$($Time.Day)" + "_" + "$($Time.Year)" + ".csv"

        #Use join-path to add path to file name
        [String] $FilePath = (Join-Path $Path $FileName)

        $Count = (Get-Process -Name php -ComputerName &lt;Server&gt; | Measure-Object).count

        [pscustomobject]@{
            Time = (Get-Date -Format HH:mm)
            Count = $Count
        } | Export-CSV $Filepath -Append

        Write-Verbose "Saved PHP Count to $FilePath. Sleeping for $Interval seconds"

        Start-Sleep -Seconds $Interval
    }

    While ((Get-Date -Format HH:mm) -lt $EndTime)
}
```

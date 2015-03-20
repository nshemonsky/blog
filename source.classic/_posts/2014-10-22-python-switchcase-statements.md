---
layout: post
title: Python switch/case statements
date: 2014-10-22
comments: true
categories:
  - SysAdmin
tags:
  - Python
  - Scripting
---

Really, this post is about the complete lack thereof. While working on a script
I needed to evaluate one of several potential arguments being passed in from
the command line. Not wanting to write an if/elif/else nested 20 levels deep I
went about doing what seems to be a fairly standard workaround for implementing
a switch statement in Python; I created a dictionary mapping to a function for
each potential option.

<!-- more -->

Let's create a simple demonstration. You could save the below code as *math.py*.

``` python Python switch/case example script
#!/usr/bin/env python2.7
import sys

operation = sys.argv[1]
num = int(sys.argv[2])

def add():
  return (num + num)

def subtract():
  return (num - num)

def multiply():
  return (num * num)

def divide():
  return (num / num)

def error():
  raise SystemExit("Unrecognized operation.")

math_op = {
  "add" : add,
  "subtract" : subtract,
  "multiply" : multiply,
  "divide" : divide,
}

print math_op.get(operation, error)()
```

When running this you'll pass in 2 arguments, the operation you want to perform;
add, subtract, multiply, or divide, and a number to use. I've also included an
error function in case you pass in an operation that isn't supported. I'm using
the dictionary get method to handle finding the appropriate operation and
defaulting to error if it's not found.

``` bash Running the script
$ ./math.py add 3
6

$ ./math.py multiply 3
9

$ ./math.py divide 3
1

$ ./math.py subtract 3
0

$ ./math.py foo 3
Unrecognized operation.
```

Although this is quite simple, it comes in handy. I hope you find it useful.
For some more elegant solutions, see the following:

  * [Readable switch construction without lambdas or dictionaries][1]
  * [Exception-based Switch-Case][2]
  * [Using a Dictionary in place of a switch statement][3]

[1]: http://code.activestate.com/recipes/410692/ "Readable switch construction without lambdas or dictionaries"
[2]: http://code.activestate.com/recipes/410695/ "Exception-based Switch-Case"
[3]: http://code.activestate.com/recipes/181064/ "Using a Dictionary in place of a 'switch' statement"

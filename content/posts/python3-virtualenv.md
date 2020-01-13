---
title: "Configuring a Python3 virtual environment"
date: 2020-01-12T13:21:47-07:00
draft: false
author: "Beamer"
---

## Overview 

Python is a fantastic programming language, with tools available for nearly any
task. Unfortunately, its popularity can also be something of a drawback. The 
majority of Linux distros use Python extensively for system configuration and
management. Modification of the system Python environment for developing 
software can cause system errors. Additionally, two or more different Python programs
may have conflicting dependencies, making it difficult to work on multiple projects
at the same time.

Virtual environments are a step in solving this. They allow the creation of self-contained
Python environments with separate packages and interpreter versions. Changes made in
these environments do not impact the system Python; and the virtual environment can
be used as part of the project's git repository, allowing everyone to share the same
dependencies.

## Creating a Python virtual environment

### Installation

The Python 3 virtual environment tools are installed via `pip3`

`pip3 install virtualenv`

This installs the `virtualenv` tool, allowing the creation of Python 3 virtual
environments.

### Creation

Creating a Python virtual environment is quite simple. The following steps
outline the creation and validation of a Python virtual environment.

Before creating the virtual environment, as a demonstration, observe the system
environment before usage of `virtualenv`.

`which python3`

Output:

`/usr/local/bin/python3`

Next create a working directory for testing the virtual environment:

`mkdir ~/python3_virtualenv`

And switch to that directory:

`cd ~/python3_virtualenv`

Before creating virtual environment, create the following script:

`touch test.py`

And insert the following text into the script:

```
#!/usr/bin/env python3

import sys

try:
    import httpx
except ModuleNotFoundError:
    print "Sorry, httpx is not installed"
    sys.exit(1)

print("We imported the httpx module!")
sys.exit(0)
```

At this point, there is no httpx module installed.

To demonstrate that we are not within the virtual environment, run the 
script:

`python3 test.py`

The expected output is:

`Sorry, httpx is not installed`

Check the exit code:

`echo $?`

The expected output is:

`1`

Finally, create the virtual environment:

`virtualenv $(pwd)/python3_virtualenv`

This creates several new directories and installs `pip`, `setuptools` and `wheel`
Note that this `pip` is the Python 3 pip, so use `pip` and not `pip3` to install 
pip packages when in the virtualenv. 

Next activate the virtual environment

`source bin/activate`

The terminal should now have the `(python3_virtualenv)` at the beginning
of the line.

When the `bin/activate` script is sourced, it replaces the environment Python 3 with
with the virtual environment Python 3 and sources the libraries within. Only pip packages
within this virtual environment are used.

Now that the virtual environment is activated, check the location of the Python interpreter

`which python`

Expected output:

`<DIRECTORY>/python3_virtualenv/bin/python3`

The default path for Python 3 is now replaced.

Now install the `httpx` library:

`pip install httpx`

Note: Make sure to use `pip` and not `pip3`

Now re-run the `test.py` script:

`python test.py`

Note: Make sure to use `python` and not `python3`

Expected output:

`We imported the httpx module!`

Check the exit code:

`echo $?`

Expected output:

`1`

Finally, deactivate the virtual environment:

`deactivate`

Note that `(python3_virtualenv)` disappears from the shell prompt

Re-run the `test.py` script:

`python3 ./test.py`

Expected output:

`Sorry, httpx is not installed`

Check the exit code:

`echo $?`

`1`

At any point in time the virtual environment can re-activated, simply by sourcing the
`bin/activate` script again:

`source path/to/script/bin/activate`

All previously installed libraries and configurations made to the virtual environment
remain present.

## Review

In summary:

1. The virtual environment is installed with `pip3 install virtualenv`

2. Create a new virtual environment with `virtualenv path/to/env`

3. Activate virtual environment with `source path/to/script/bin/activate`

4. Deactivate the virtual environment with `deactivate`

Python virtual environments are extremely useful for keeping configuration and packages
separate from the system Python. Virtual environments should be stored as part of the project's
git repository. This allows everyone to use the same Python environment and eliminates confusion
of developers installing different versions of packages and/or Python interpreters.

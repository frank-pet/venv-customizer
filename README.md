## venv-customizer

## Overview

Python Virtual Environments are "mandatory" with python 3.11 and Debian
Bookworm.  The python documentation for virtual environments implemented with 
*venv* and is documented [in the python docs][docs.python].  While extending venv is discussed it is not
obvious and requires writing a subclass of venv.builder.  This situation begs
for an easy solution.  

## Approach

The simplest extensions this package provides is the abiliy to define additional
environment variables and functions to support exection in the venv.  The 
most obvous and pressing example is using pylint in a venv is that pylint fails to use
the venv version of pylint.  Imports fail when pylint does its checking because it is not using the venv version of python.  The
soluton is to alias pylint to use the correct version of pylint:

`alias pylint='"${VIRTUAL_ENV}/bin/python" /usr/bin/pylint'`

Other examples include definng other environment variables or changing to the
desired initial directory.  

This motivates an approach for virtual environment extension, comprising the
abiltiy to execute ('source', actually) shell files before and/or after a 
venv is activated, and before and/or after a venv is deactivated.  It additionally
provides the opportunity to provide a corresponding "activate" function for the
shell environment to avoid having to type "source venv/bin/activate". 

The customization can occur both system-wide and at the project level.  The developer
can povide post-activate and post-deactivate scripts for each project that are
sourced from the system wide post-activate and post-deactivate.

## Installation

Download the repository either via creating a working directory and either
execute'git clone https://github.com/frank-pet/venv-customizer.git' or 
by download and unpack the file.  After this it is a three step process:

1. move the directory .venv.d to your login directory on linux,
2. move files in the venv directory to your project venv directory and either:
    * source the venv-functions file from your .bashrc or .bash_aliases file, or
    * copy the code in the venv-functions to the end of your .bashrc or .bash_aliases file.
3.  customize the pre/post-activate/deactivate files in .venv.d directory for customizations that apply to all projects, or to the `post-activate` or `post-deactivate` file in the project venv directories.

If pre-activate/deactive customizations are required at the project level, the changes needed to the pre-activate/deactivate files in venv can be modeled after the chanes in the post-activate/deactive files.
Note that the customization files in the repository have commands to enable pylint to function correctly in venvs commented out.

After a successful installation, command(function) `activate` can be used to activate the virtual environment; `deactivate` will continue to deactivate the virtual environment.

[docs.python]: https://docs.python.org/3/library/venv.html

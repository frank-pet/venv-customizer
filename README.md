## venv-extender

## Overview

Python Virtual Environments are "mandatory" with python 3.11 and Debian
Bookworm.  The python documentation for virtual environments implemented with 
*venv* and is documented [here].  While extending venv is discussed it is not
obvious and requires writing a subclass of venv.builder.  This situation begs
for an easy solution.  

## Approach

The simplest extensions this package provides is the abiliy to define additional
environment variables and functions to support exection in the venv.  The 
most obvous and pressing example is using pylint in a venv.  pylint fails to use
the venv version of pylint, so imports fail when pylint does its checking.  The
soluton is to alias pylint to use the correct version of pylint:

`alias pylint='"${VIRTUAL_ENV}/bin/python" /usr/bin/pylint'`

Other examples include definng other environment variables or changing to the
desired initial directory.  

This motivates an approach for virtual environment extension, comprising the
abiltiy to execute ('source', actually) of shell files before and/or after a 
venv is activated, and before and/or after a venv is deactivated.  It additionally
provides the opportunity to provide a corresponding "activate" function for the
shell environment to avoid having to type "source venv/bin/activate". 

The customization occurs both system-wide and at the project level.  The user
can povide post-activate and post-deactivate scripts for each project that are
sourced from the system wide post-activate and post-deactivate.

## Installation







[docs.python]: [https://docs.python.org/3/library/venv.html]

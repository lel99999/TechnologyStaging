---
layout: default
title: "Setting Up Python Development Envioronments"
parent: "Documentation"
---

### Setting Up Python 2.x & 3.x on OSX

Prerequisites:
homebrew

Check if homebrew is installed:
'''$which brew
/Users/<yourID>/homebrew/bin/brew
'''

#### Install Python

Install the latest version of python 2.7:
'$brew install python'

Install the latest version of python 3.x:
'$brew install python3'

#### Install virtualenv and virtualenvwrapper
Virtualenv is a tool that lets you create isolated Python environments for your projectrs.  It creates an environment that has its own installation directories, that doesn't share dependencies with other 'virtualenv' environments (and optionally doesn't access the globally installed dependencies either).  Virtualenvwrapper is an extension to 'virtualenv' which provides a set of friendly commands that helps simplify an otherwise obtus virtualenv.  It makes it easier to work on multiple projects and helps simplify the creation and deletion of virtual environments without creating dependency conflicts.

To install virtualenv and virtualenvwrapper:
'''
$pip install virtualenv
$pip install virtualenvwrapper

'''

#### Update and set environment persistence in your bash profile

'''
1. # to activate virtualenvwrapper
1. source /usr/local/bin/virtualenvwrapper.sh
1. 
1. # pip should only run if a virtualenv is currently activated
1. export PIP_REQUIRE_VIRTUALENV=true
1. 
1. # commands to override pip restrictions
1. # use `gpip` or `gpip3` to force installation of packages in the global python environment
1. gpip(){
1.   PIP_REQUIRE_VIRTUALENV="" pip "$@"
1. }
1. 
1. gpip3(){
1.   PIP_REQUIRE_VIRTUALENV="" pip3 "$@"
1. }
'''

Save the file, then to update your current environment with the changes:

'$. ~/.bash_profile'

You have activated virtualenvwrapper and locked down pip to work only inside a virtual environment, which protects you from accidentally installing python packages to the global python environment.  However, you have also provided an override, when the necessity to install a python package globally exists (rarerly!!), you can use 'gpip' or 'gpip3' to override the protection.

#### Start Development

Create projects 

'''

# python 2.7
$mkvirtualenv pyproject

# python 3.x
$mkvirtualenv -p python3 pyproject

'''

You should see the name of the virtual environment in the prompt indicating it is safe to install packages with pip

'''

(pyproject) devcomputer:~$

'''

To install any packages that are needed for the project (NOTE: separate multiple pacages with a space)

'''

# python 2.7
pip install wsgiref boto

# python 3.x
pip3 install wsgiref boto

'''

To check, which packages have been installed, use 'pip freeze'

'''

$pip freeze
boto==2.3.0
wsgiref==0.1.2

'''

#### To exit the virtual environment

'$deactivate'

#### To return to a previous project

'$workon pyproject'

#### To remove a virtual environment and all packages installed

'$rmvirtualenv pyproject'

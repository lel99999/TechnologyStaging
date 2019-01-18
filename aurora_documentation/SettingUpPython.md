---
Layout: default
Title: "Setting Up Python Development Envioronments"
Parent: "Documentation"
---

### Setting Up Python 2.x & 3.x on OSX

Prerequisites:
homebrew

Check if homebrew is installed:

```
$which brew
/Users/<yourID>/homebrew/bin/brew
```

#### Install Python

Install the latest version of python 2.7:

```
$brew install python
```

Install the latest version of python 3.x:

```
$brew install python3
```

#### Install pip

Python 2 > 2.7.8 and Python 3 > 3.3  (i.e. Python 2 -> 2.7.9 or greater/Python 3 -> 3.4 or greater) ship with pip.

#### Install virtualenv and virtualenvwrapper
Virtualenv is a tool that lets you create isolated Python environments for your projectrs.  It creates an environment that has its own installation directories, that doesn't share dependencies with other 'virtualenv' environments (and optionally doesn't access the globally installed dependencies either).  Virtualenvwrapper is an extension to 'virtualenv' which provides a set of friendly commands that helps simplify an otherwise obtus virtualenv.  It makes it easier to work on multiple projects and helps simplify the creation and deletion of virtual environments without creating dependency conflicts.

To install virtualenv and virtualenvwrapper:
```
$pip install virtualenv
$pip install virtualenvwrapper
```

#### Update and set environment persistence in your bash profile

```
1.  # to activate virtualenvwrapper
2.  source /usr/local/bin/virtualenvwrapper.sh
3. 
4.  # pip should only run if a virtualenv is currently activated
5.  export PIP_REQUIRE_VIRTUALENV=true
6. 
7.  # commands to override pip restrictions
8.  # use `gpip` or `gpip3` to force installation of packages in the global python environment
9.  gpip(){
10.   PIP_REQUIRE_VIRTUALENV="" pip "$@"
11. }
12. 
13. gpip3(){
14.   PIP_REQUIRE_VIRTUALENV="" pip3 "$@"
15. }
```

Save the file, then to update your current environment with the changes:

```
$. ~/.bash_profile
```

You have activated virtualenvwrapper and locked down pip to work only inside a virtual environment, which protects you from accidentally installing python packages to the global python environment.  However, you have also provided an override, when the necessity to install a python package globally exists (rarerly!!), you can use 'gpip' or 'gpip3' to override the protection.

#### Start Using/Development

##### Create projects 

```
# python 2.7
$mkvirtualenv pyproject

# python 3.x
$mkvirtualenv -p python3 pyproject
```

You should see the name of the virtual environment in the prompt indicating it is safe to install packages with pip

```
(pyproject) devcomputer:~$
```

To install any packages that are needed for the project (NOTE: separate multiple packages with a space)

```
# python 2.7
$pip install wsgiref boto

# python 3.x
$pip3 install wsgiref boto
```

To check, which packages have been installed, use 'pip freeze'

```
$pip freeze
boto==2.3.0
wsgiref==0.1.2
```

##### To exit the virtual environment

```
$deactivate
```

##### To return to a previous project

```
$workon pyproject
```

##### To remove a virtual environment and all packages installed

```
$rmvirtualenv pyproject
```

## Finer Grained Management of Multiple Versions of Python
To be able to switch quickly between different versions of Python, use [PyEnv](https://github.com/pyenv/pyenv)

#### Install Pyenv
```
$brew update
$brew install pyenv
```
#### Install a specific version of Python
Review [available Python 2 & 3 versions](https://www.python.org/downloads/)
```
$pyenv install 2.7.13
$pyenv install 3.7.2

### check the current python version
$pyenv version

### check all the python versions installed by pyenv
$pyenv versions
* system (set by /Users/.../.pyenv/version)
2.7.12
3.7.2
```
#### Set Global Python Version
This sets the default version of Python
```
$pyenv global 3.7.2
```

#### Setting a local Python version
For projects that require legacy versions, go to the project folder and execute the following command:

```
$pyenv local 2.7.12
````
This creates a hidden file **.python-version**, which contains the specific version of Python required to execute the project.
If a Python script is executed outside this specific folder, the Python version that will be used will be the global version



## Setting up Python 2.x & 3.x on Windows 10

#### Install Python

Go to Python download site:
Python 2.7.9 -> [link](https://www.python.org/downloads/release/python-279/)
[Download Windows x86_64 executable installer](https://www.python.org/ftp/python/2.7.15/python-2.7.15.amd64.msi)

Python 3.6.8 -> [link](https://www.python.org/downloads/release/python-368/)
[Download Windows x86-64 executable installer](https://www.python.org/ftp/python/3.6.8/python-3.6.8-amd64.exe)


#### Install pip

Python 2 > 2.7.8 and Python 3 > 3.3  (i.e. Python 2 -> 2.7.9 or greater/Python 3 -> 3.4 or greater) ship with pip. 

#### Install virtualenv and virtualenvwrapper-win

```
C:\Users\devuser\pip install virtualenv
```

Now  virtualenv is installed which allows you to create individual environments.  But to manage all those environments, you will also need the helper package virtualenvwrapper.

```
C:\Users\devuer\pip install virtualenvwrapper-win
```

#### Start Using/Development
```
1.  Make a Virtual Environment
2.  Connect our project with our Environment
3.  Set Project Directory
4.  Deactivate
5.  Workon
6.  Pip Install <packages>
```

#####  Create Project

```
C:\Users\devuser\mkvirtualenv pyproject
```

This will create a folder with python.exe, pip, and setuptools in its own environment. It will also activate the Virtual Environment which is indicated with the (pyproject) on the left side of the prompt.

##### To exit the virtual environment

```
(pyproject) C:\Users\devuser\deactivate
````

##### To return to a previous project

```
C:\Users\devuser\workon pyproject
```


## Setting up Python 2.x & 3.x on Linux (CentOS/RedHat 7.x)
***NOTE*** As yum is an important package manager for CentOS/RedHat, and built with Python 2.6, the process of installing Python requires caution as it may break yum and certain dependencies. Therefore it is recommend to use Software Collections

***Software Collections (SCL)*** provides the power to build, install and use multiple versions of software on the same system without affecting system-wide installed packages.  The SCL packaging technique is frequently used for building stacks for Red Hat Enterprise Linux and CentOS, especially dynamic languages (Python, Ruby, NodeJS) or databases (PostgreSQL, MariaDB, MongoDB)

The SCL technique is based on avoiding conflicts on a number of levels:
- Filesystem (files are put into an alternate directory under /opt/rh)
- RPM package name(all packages names are prefixed by the collection name)
- RPM metadata (key consideration is name uniqueness, so SCL packages do not influence the packages from base system)

To use SCL packages, users need to add a few steps to the usual interaction with RPMs to enhance the interaction, namely, they need to use ***scl enable*** call which changes the environment varialbes like ***PATH*** or ***LD_LIBRARY_PATH***, so that binaries under alternate paths are found.  

### Installation Prerequisites
1. Install development tools including GCC, make, and git
```
$su -
#yum install @development
```
2. Enable repos with additional developer tools
While the base RHEL software repos have many development tools, older versions that ship with the OS and are supported for the full 10-year life of the OS, there are packages that have a different support lifecycle that are not enabled by default.

Red Hat software Collections are in the rhscl repo.  RHSCL packages have some dependencies on packages in the optional-rpms rep, so both will need to be enabled.

To enable the additional repos, run the following commands as ***root***:

### CentOS 7.x
For Python 2.7.x & 3.x
```
$sudo yum install centos-release-scl
```

### RHEL 7.x
For Pythonn 2.7.x
```
$su -
#subscription-manager repos \
--enable rhel-7-server-optional-rpms --enable rhel-server-rhscl-7-rpms
```
For Python 3.x
```
$sudo yum config-manager --enable rhel-server-rhscl-7-rpms
$sudo yum config-manager --enable rhel-server-rhscl-beta-7-rpms
```

### Install the collection
### CentOS/RHEL 7.x

For Python 2.7.x
```
$sudo yum install python27
```

For Python 3.6.x
```
$sudo yum install rh-python36
```

### Start using software collections
For Python 2.7.x
```
$scl enable python27 bash
```
For Python 3.6.x
```
$scl enable rh-python36 bash
```

***You should be able to use python just as a normal application***
```
$python dev-app.py
$pip install --user Flask
$pip install --user Django
```



### Python Software Collection as Docker Formatted Container
### CentOS/RHEL 7.x
For Python 2.7.x
```
$docker pull rhscl/python-27-rhel7
```
For Python 3.6.x
```
$docker pull rhscl/python-36-rhel7
```


## Using Anaaconda

---
layout: post
title: 'Managing Virtual Environments'
description: "Managing Virtual Environments"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2019/virtual_environment2.jog
featured_image: assets/images/posts/2019/virtual_environment2.jpg
featured: false
hidden: false
---

Python, like most other modern programming languages, has its own unique way of downloading, storing, and resolving packages (or [modules](https://en.wikipedia.org/wiki/Modular_programming)). While this has its advantages, there were some interesting decisions made about package storage and resolution, which has lead to some problems—particularly with how and where packages are stored.

There are a few different locations where these packages can be installed on your system. For example, most system packages are stored in a child directory of the path stored in [sys.prefix](https://docs.python.org/3/library/sys.html#sys.prefix).

On Mac OS X, you can easily find where `sys.prefix` points to using the Python shell:

```python
import sys
sys.prefix
'/System/Library/Frameworks/Python.framework/Versions/3.5'
```

The following are just notes I wrote down (or copied) for myself while switching from anaconda to `virtualenv`.

[`Anaconda`](https://www.anaconda.com) has its own python distribution, package manager, installer and virtual environment tool. But if you are not a Data Scientist, anaconda is most likely an overkill. After all, anaconda will install everything it has to offer. Which is over 8 GB of disk space.

## Problem

Problems with system-wide installs:
* Multiple projects with conflicting dependencies
* Conflicts with system dependencies
* Multi-user systems
* Testing code against different python and library versions

## Virtualenv

Install globally `sudo python -m pip install virtualenv` (Python 2.7). With Python 3.3 or higher, we can use `venv` instead and don't need to install anything.

To create a project, we need first a directory to host the virtualenv projects

```python
mkdir virtualenvs  # this can be also a hidden directory, which by default it is
cd virtualenvs

virtualenv <project name>  # Python 2.7
python -m venv <project name>  # Python 3.3 or higher
```

When we look inside that directory we will find several directories: `bin`, `include`, `lib` and `local`. In the bin directory we can find `activate`, `pip` and `python`. The latter two commands are dedicated versions for that project we created. The command `activate` activates the virtual environment for that project.

But before we go into that, I wanted to make sure that both python 2.7 and 3.x are covered. In order to make sure that your project has the correct python version, we run virtualenv as follows:

```python
virtualenv -p python3 <project name>
```

Working inside a virtual environment
- Activating the environment
    ```
    . <project name>/bin/activate
    ```
    The dot `.` means that we will first import shell scripts from the bin directory in the project directory.
- Running python and pip
- Installing a package
- Deactivate

### Projects and Virtualenvs

+ Projects
    - Contain source code
    - Are under version control


+ Virtual Environments
    - Contain packages, tools, python, etc.
    - Keep them separate from your projects
    - Usually one venv per project


### requirements.txt

This file allows you to make sure that others use the same packages as you are and that file should be under version control. To create that file you need to run

```
python -m pip freeze > requirements.txt
```

And to install all the dependencies listed in requirements.txt

```
python -m pip install -r requirements.txt
```

### Specifying Versions

We can specify the version of a package as follows:
- package must be a specific version: `docopt == 0.6.1`
- package should be of a minimum version: `keyring >= 4.1.1`
- package can be anything except of a given version: `coverage != 3.5`

We can install specific versions using pip directly:

```
python -m pip install flask==0.9
```

But make sure if you want to install for example any package lower than 2.0, you need to use quotes:

```
python -m pip install 'Django<2.0'
```

To upgrade a package we can run

```
python -m pip install -U flask
```

---

## Virtualenvwrapper

- A user-friendly wrapper around virtualenv
- Easy creation and activation
- Bind projects to virtualenvs
- Great with large numbers of projects

Before we can use it, we need to add some lines to `bash`, or in my case `.zshrc`:

```zsh
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Projects
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
export VIRTUALENVWRAPPER_SCRIPT=/usr/local/bin/virtualenvwrapper.sh
source /usr/local/bin/virtualenvwrapper_lazy.sh
```

- List all projects under virtualenvs: `workon`
- Activate a project: `workon <project name>` (we can use `pip list` for example to see which packages are installed)
- Create a project directory and the virtualenvs directory at the same time (activates the newly created project and jumps to it): `mkproject <project name>` or `mkproject -p python3 <project name>`

If you have a project which didn't got created using `mkproject`, we can use `setvirtualenvproject <project name>` to link the project with a virtualenv directory. This binds an existing project to a virtualenv, and binds active venv to the current working directory (which should be your project).

- Create a virtualenv: `mkvirtualenv <new env>`
- Remove a virtualenv: `rmvirtualenv <some env>`
- Create a temporary virtual environment: `mktmpenv`


## Note

In order to make sure that there is a proper work flow in regards to managing projects and its packages, python has created the "Python Packaging Authority" which can be found at `https://pypa.io`.

## Managing Packages

There are several newer tools we can use here, for example, `pipenv` or `poetry`.

### [pipenv](https://pipenv.readthedocs.io/en/latest/)

The problems that Pipenv seeks to solve are multi-faceted:

- You no longer need to use `pip` and `virtualenv` separately. They work together.
- Managing a `requirements.txt` file [can be problematic](https://www.kennethreitz.org/essays/a-better-pip-workflow), so Pipenv uses `Pipfile` and `Pipfile.lock` to separate abstract dependency declarations from the last tested combination.
- Hashes are used everywhere, always. Security. Automatically expose security vulnerabilities.
- Strongly encourage the use of the latest versions of dependencies to minimize security risks [arising from outdated components](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities).
- Give you insight into your dependency graph (e.g. $ `pipenv graph`).
- Streamline development workflow by loading `.env` files.

Here, we can just go into our project and run the following command to install packages and create a virtualenv:

```
pipenv install request python-box
```

It will
- create a virtual environment under `~/.local/share/virtualenvs/`
- installs the dependencies
- creates the Pipfile and Pipfile.lock files to record our requirements and dependencies.

To "activate" the project we run `pipenv shell` and to leave it we use `exit`.


### [poetry](https://poetry.eustace.io/)

- pyproject.toml
- standard (PEP-518)
- deterministic


`poetry new <project name>`: This creates a README.rst and pyproject.toml file as well as the project and tests directory.

`poetry add <package name>`: installs packages

And similarly to pipenv, we run `poetry shell` to activate our project and `exit` to leave it.


### [direnv](https://direnv.net/)

`direnv` is an extension for your shell. It augments existing shells with a new feature that can load and unload environment variables depending on the current directory.

Here is a quick follow along demo:

```zsh

# Create a new folder for demo purposes.
$ mkdir ~/my-project
$ cd ~/my-project

# Show that the FOO environment variable is not loaded.
$ echo ${FOO-nope}
nope

# Create a new .envrc. This file is bash code that is going
# to be loaded by direnv.
$ echo export FOO=foo > .envrc
.envrc is not allowed

# The security mechanism didn't allow to load the .envrc.
# Since we trust it, let's allow it's execution.
$ direnv allow .
direnv: reloading
direnv: loading .envrc
direnv export: +FOO

# Show that the FOO environment variable is loaded.
$ echo ${FOO-nope}
foo

# Exit the project
$ cd ..
direnv: unloading

# And now FOO is unset again
$ echo ${FOO-nope}
nope

```



## Notes to self

`direnv` seems a very convenient to negative between projects, by automatically activating and deactivating a virtual environment.


`poetry` has a nicer "UI" and it supports packaging. One thing I am not a very big fan right now, that it __Created package <span style="color:lightgreen">my_project</span> in <span style="color:blue">my_project</span>__. Which results in the following directory structure:

```
$ cd my_project
$ tree
.
├── README.rst
├── my_project
│   └── __init__.py
├── pyproject.toml
└── tests
    ├── __init__.py
    └── test_my_project.py
```


`pipenv` seems like the most popular.


### Extra

[`tox`](https://tox.readthedocs.io/en/latest/) aims to automate and standardize testing in Python. It is part of a larger vision of easing the packaging, testing and release process of Python software.

---

## Feature Comparison

The following is a copy from <a href="https://stackoverflow.com/a/58218593/5983691" target="_blank">stackoverflow</a> _from Oct 3rd, 2019_.


| Feature \ Package Manager             | npm[^1] | pip | pipenv       | poetry         |
|---------------------------------------|---------|-----|--------------|----------------|
| `Access` to main repo (i.e. Pypi/npm) | ✓       | ✓   | ✓            | ✓              |
| `Record` top level dependencies       | ✓       | ✗   | Pipfile      | pyproject.toml |
| `Record` development dependencies     | ✓       | ✗   | Pipfile      | pyproject.toml |
| `Lock` versions of all dependencies   | ✓       | ✓   | Pipfile.lock | poetry.lock    |
| `Switch` between interpreter versions | nvm     | ✗   | ✗            | ✓              |
| `Direct` publishing                   | ✓       | ✗   | ✓*           | ✓              |
| `Run` scripts                         | ✓       | ✗   | Pipfile      | ✗              |
| `Editable` local packages             | ✓       | ✓   | ✓            | ✓              |
| `Integration` with Intellij           | ✓       | ✓   | partial      | ✗              |

[^1]: npm is a js/node package manager

<!-- <details>
  <summary>
    <a class="btnfire small stroke"><em class="fas fa-chevron-circle-down"></em>&nbsp;&nbsp;
        Click to read the whole text from stackoverflow here!
    </a>
  </summary> -->

### Basic Usage

#### pipenv

To get the most out of `pipenv`, `pyenv` should be installed. `pipenv` will be able to detect and use any version of python installed with `pyenv`, even if it is not activated. For example if a `Pipfile` has listed python 3.4 as a requirement: to successfully run `pipenv install`, `pyenv install 3.4.0` should be run first.

To create a new venv (using python 3.7.x) and `Pipfile`:
```
pipenv --python 3.7
```
Or to install dependencies from an existing Pipfile.lock use the command below. This command can also be used to create a Pipfile and venv (defaulting to the newest available python version).
```
pipenv install
```
To run commands within the created venv:
```
pipenv run <script or command>
```
e.g
```
pipenv run python main.py
```

##### poetry

Poetry still uses `pyenv` but in a different way: The version of python you wish to use must be activated before calling `poetry install` or `poetry run`.

A `pyproject.toml` can be created using:
```
poetry init
```
or a full directory structure can be created using:
```
poetry new <dir>
```
Before we can install we must activate a version of python that matches what is specified in the pyproject.toml file.
```
pyenv global <python version specified in pyproject.toml>
```

Now we are able to create the venv using the command below, if a poetry.lock file is present it will install all the dependencies listed in it.
```
poetry install
```
To run commands within the created venv:
```
poetry run <command>
```
If we change the global python version using `pyenv` we will no longer be able to run commands in the created venv. There is an exception to this if we use a locally created venv, see below.


### Running your code using different python versions

Sometimes it's good to check that your code will work on both python 3.7 and python 3.4. This is not something we can take for granted.

##### pipenv

Only possible by deleting the venv in recreating it using a different python version:
```
rm -rf <path to venv>
pipenv --python <different python version>
```
A warning will likely be displayed that the python version of the venv does not match the one specified in the Pipfile, but as far as I can tell it is only a warning.

See GitHUb issue [here](https://github.com/pypa/pipenv/issues/1071)

##### poetry

Poetry is much better suited to this use case: Multiple venvs can be created side by side. To create and use a new venv switch python versions using pyenv then create a new venv.
```
pyenv global <different python version>
poetry install
```

An error will be thrown if the python version does not match the one specified in the pyproject.toml however a range of python versions can be specified using semver versioning.

### Local venv

I prefer my venv to be installed in a `.venv` folder local to my project, this is similar to how `npm` works and allows me to delete the folder and reinstall if anything strange happens or if (in the case of pipenv) I want to easily change which version of python I'm using.

##### pipenv

To enable this feature set the following environment variable.
```
export PIPENV_VENV_IN_PROJECT="enabled"
```

##### poetry

This feature can be enabled using the following command:
```
poetry config settings.virtualenvs.in-project true
```

But beware that this will change the behavior of `poetry`, it will no longer be possible to use quickly switch between different versions of python: Even if the python version is switched using `pyenv` all commands run using `poetry run` will use the venv (and its associated python version) that resides in the local directory.

See GitHUb issue [here](https://github.com/sdispater/poetry/issues/108).


### Installing Packages

##### pipenv

Packages are easily installed and automatically added to the `Pipfile` and `Pipfile.lock` files using:
```
pipenv install [--dev] <package name>
```
The `--dev` flag indicates a development dependency. Development dependencies will not be installed by default when using `pipenv install`.

Local packages can also be installed, allowing you to work on them and see you changes immediately:
```
pipenv install -e <path to local package>
```

##### poetry

Packages are easily installed and automatically added to the `pyproject.toml` and `poetry.lock` files using:

```
poetry add [--dev] <package name>
```

The `--dev` flag indicates a development dependency, Development dependencies will not be installed by default when using poetry install or added to the package when publishing.

Local packages can also be installed, allowing you to work on them and see you changes immediately:
```
poetry add --path <path to local package> <name of package>
```

Not sure why the name of the package is needed, as it should already be defined by the local package. [Also the author seems unconvinced about linking local packages in general](https://github.com/sdispater/poetry/issues/34) so this feature may get forgotten about over time.

### Running Scripts

To be clear, I'm referring to what npm calls scripts, which is different to the scripts specified inside a `setup.py` file.

When developing it is sometimes useful to set up shortcuts for commands that are difficult to remember, for example the command for running every test file in a directory is:
```
python -m unittest discover -s <test_folder> -p '*_test.py'
```

It is much more convenient to have a shortcut to these sorts of commands.

##### pipenv

This feature is supported: put the following into the `Pipfile`:

```
[scripts]
    test = "pipenv run python -m unittest discover -s tests -p '*_test.py'"
```

##### poetry

Not supported, and unlikely to be added in the [future](https://github.com/sdispater/poetry/pull/591#issuecomment-504762152).

### Publishing to PyPi

It would be preferable to be able to publish to PyPi without crafting an additional `setup.py` file, this would be possible if all the information needed to publish the package was contained within the package management files.

##### pipenv

As far as I can tell this is where pipenv gets its bad reputation. `setup.py` files are still needed to publish to PyPi and no, they are not auto-populated with the dependencies from the `Pipfile`.

The recommended approach is to either copy the dependencies over manually when publishing, or to get the Pipfile to install the dependencies listed in the `setup.py` files, however, the `setup.py` is not automatically updated when running `pipenv install <package name>`.

If you really want your `Pipfile` to depend on your `setup.py` file, this is how it's done:
```
pipenv install '-e .'
```

But there are some issues:
+ [https://github.com/pypa/pipenv/issues/2805](https://github.com/pypa/pipenv/issues/2805)
+ [https://realpython.com/pipenv-guide/#yes-i-need-to-distribute-my-code-as-a-package](https://realpython.com/pipenv-guide/#yes-i-need-to-distribute-my-code-as-a-package)
+ [https://github.com/pypa/pipenv/issues/209](https://github.com/pypa/pipenv/issues/209)

Yuck!

---

So ideally we want to derive a setup.py file from the `Pipfile`:

I found two existing packages that claim to do this:

[https://pypi.org/project/pipenv-tools/](https://pypi.org/project/pipenv-tools/) - But it hasn't been updated in two years, there's no code in the `src` directory and I couldn't get it to work.

[https://pypi.org/project/pipenv-setup/](https://pypi.org/project/pipenv-setup/) - But it syncs the `Pipfile.lock` instead of the `Pipfile`, this is an anti-pattern. The lock file is meant for creating a reproducible environment, it is overly restrictive (e.g. by not allowing updates to dependencies) to be used for `setup.py`. For this reason I didn't even try using it.

---

My Solution:

I quickly wrote a package that generates an `install_requires.py` file that can be imported in a `setup.py` file: [https://pypi.org/project/pipenv2setup/](https://pypi.org/project/pipenv2setup/) (it is untested on Windows).

For an example of how to use the package when publishing pipenv projects, see this github repo:

[https://github.com/alanbacon/pipenvExample](https://github.com/alanbacon/pipenvExample)

##### poetry

Publishing your package using poetry is really easy, you do not need a `setup.py` file at all. Simply run:

```
poetry publish [--build] [--username <username>] [--password <password>]
```

The published package can be installed using pip not just by other instances of poetry.

For information about how to migrate from using a setup.py to purely a pyproject.toml file, see [here](https://johnfraney.ca/posts/2019/05/28/create-publish-python-package-poetry/)

### IntelliJ or Pycharm integration

##### pipenv

Pycharm is able detect the venv by using the `Piplock` files, however adding new packages using the Pycharm interface will not modify the `Piplock` files.

##### poetry

At the time of writing Pycharm does not seem to be aware of any poetry virtual environments or appear to parse the `pyproject.toml` file in any way.

### Other Points about poetry

#### Semver

In `poetry` you must specify versions of python and packages using [semver](https://nodesource.com/blog/semver-tilde-and-caret) (have to use `~` and `^`, not `>=` or `<`).

#### Buggyness

`poetry` runs using python but doesn't work with older versions of python. So to develop for an older version of python: some commands have to be run with `pyenv` set to >3.6, but then the `pyenv` needs to be switched back to the older version to create the venv. [It also appears that venvs have to be greater than 3.5.](https://github.com/sdispater/poetry/issues/1223)

Also not sure about windows compatibility for `poetry`.

#### Conclusion

To me the main differences between the `poetry` and `pipenv` lie in their usage of `pyenv` and their abilities (or lack of) to publish straight to PyPi. There is also the lack of scripts in poetry which I personally find frustrating.

I find that when using poetry there is a lot more switching between python environments using `pyenv`. Although currently this can be mitigated by using a locally install venv. I know this limits my ability to quickly test my code in different python environments, but there are other tools such as `tox` to do that.

Publishing to `PyPi` using `poetry` is so easy, I explained it in one line (above). Publishing to `PyPi` with `pipenv` is a minefield, to explain it I had to link to an entire git repo (above).

<!-- </details> -->

## Links

* [python environment](https://realpython.com/effective-python-environment/)
* [pipenv](https://realpython.com/pipenv-guide/)

---

##### Footnotes

<br>

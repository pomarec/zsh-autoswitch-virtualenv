:tada: Pull Requests for fixes or improvements are welcome! :tada:

Autoswitch Python Virtualenv
============================

*zsh-autoswitch-virtualenv* is a simple ZSH plugin that switches python virtualenvs automatically as
you move between directories.

You do this by simply calling the `mkvenv` command in the directory you wish to setup a virtual
environment. After that command is completed, everything is setup and you are good to go! See the
*Commands* section below for more detail.

Moving out of the directory will automatically deactivate the virtual environment. However you can
also switch to a default python virtual environment instead by setting the `AUTOSWITCH_DEFAULTENV`
environment variable.

Internally this plugin simply worksby creating a file named `.venv` which contains the name of the
virtual environment created (which is the same name as the current directory but can be edited if
needed). There is then a precommand hook that looks for a `.venv` file and switches to the name
specified if one is found.

**NOTE**: you may want to add `.venv` to your `.gitignore` in git projects (or equivalent file for
the Version Control you are using).

Requirements
------------

`virtualenvwrapper` must be installed for this plugin to work correctly.

On Ubuntu simply install from the standard repositories:

`sudo apt-get install virtualenvwrapper`

Mac OSX users can install `virtualenvwrapper` with brew:

`brew install virtualenvwrapper`

Installing with Antigen/Zgen
----------------------------

Installing with [Antigen](https://github.com/zsh-users/antigen) is super easy! Just add the following line to your `.zshrc`:

```
antigen bundle MichaelAquilina/zsh-autoswitch-virtualenv
```

Installing with [Zgen](https://github.com/tarjoilija/zgen) is also easy! Add the following line to your `.zshrc`:

```
zgen load MichaelAquilina/zsh-autoswitch-virtualenv
```

Commands
--------

Setup a new project with virtualenv autoswitching using the `mkvenv` helper command.
```
$ cd my-python-project
$ mkvenv
Using real prefix '/usr'
New python executable in /home/michael/.virtualenvs/my-python-project/bin/python2
Also creating executable in /home/michael/.virtualenvs/my-python-project/bin/python
Installing setuptools, pip, wheel...done.
Found a requirements.txt. Install? (Y/n):
Collecting requests (from -r requirements.txt (line 1))
  Using cached requests-2.11.1-py2.py3-none-any.whl
Installing collected packages: requests
Successfully installed requests-2.11.1
```
`mkvenv` will create a virtual environment with the same name as the current directory, suggest
installing `requirements.txt` if available and create the relevant `.venv` file for you.

Next time you switch to that folder, you'll see the following message
```
$ cd my-python-project
Switching virtualenv: my-python-project  [Python 3.4.3+]
$
```

If you have set the `AUTOSWITCH_DEFAULTENV` environment variable, exiting that directory will switch
back to the value set.
```
$ cd ..
Switching virtualenv: mydefaultenv  [Python 3.4.3+]
$
```

Otherwise, `deactivate` will simply be called on the virtualenv to switch back to the global
python environment.

You can remove the virtual environment for a directory you are currently in using the `rmvenv`
helper function:
```
$ cd my-python-project
$ rmvenv
Switching virtualenv: mydefaultenv  [Python 2.7.12]
Removing myproject...
```
This will delete the virtual environment in `.venv` and remove the `.venv` file itself. The `rmvenv`
command will fail if there is no `.venv` file in the current directory:
```
$ cd my-non-python-project
$ rmvenv
No .venv file in the current directory!
```

Setting a default virtualenv
----------------------------

If you want to set a default virtual environment then you can also export `AUTOSWITCH_DEFAULTENV` in
your `.zshrc` file.

```
export AUTOSWITCH_DEFAULTENV="mydefaultenv"
antigen bundle MichaelAquilina/zsh-autoswitch-virtualenv
```

Options
-------

Right now the only option available is to prevent verbose messages from being displayed when moving
between directories. You can do this by setting `AUTOSWITCH_SILENT` to a non-empty value.

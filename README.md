# The MONK Development Environment

This project aims to solve a simple problem: If you have a lot of terminal windows/tabs open every time you start working, but you have to manually start them every time you start your computer than this is for you.

The solution proposed here uses a terminal multiplexer called tmux. This tool has many cool features, but the most important one for our problem is that you can script it. And because even that script might only change in some variables, why not write a tool that does the scripting and just change some configuration if the environment changes? Exactly that's what's this tool does.

# Don't we have that tool already?

Yeah well, there's [tmuxinator](https://github.com/tmuxinator/tmuxinator). In some regards it'll even do more for you than monk_de. A very simple comparison would be that: If you have a lot of Ruby tools or code a lot of ruby yourself, then [tmuxinator](https://github.com/tmuxinator/tmuxinator) is probably the tool for you. Have a look. But if you are a Python coder, and need a lot of virtualenv handling around, then you might get problems with [tmuxinator](https://github.com/tmuxinator/tmuxinator). First of all, gem install needs you to get set up an environment a little bit like virtualenvs, but you don't know anything about that yet. Then of course you want it to set up virtualenvs for you and you need to script that yourself. Might work out, or might not.

# Okay, how do I get it working?

First of all, this tool is still in development. It is developed on Linux Ubuntu 15.04 and Python3. If you have another sytem you might experience problems. Sorry for that. We'll figure it out together if you like.

The first step as with any other python tool will be: `pyvenv install monk_de`.

What you need is an `.ini` file. For each window you want to create inside a tmux session, you have to write a separate *section*. The section name should be how you want to name your window. Then each section has three properties: *venv*, *repo*, and *path*.

*path* is the name of the directory the code will be checked out to. *repo* is the URL that `git clone` can make use off. *venv* is the name of the virtualenv that will be created.

Additionally you can add *post*, which is a shell command you can execute after everything is prepared. I found that I want to execute more shell commands than one. Currently that's possible with separating them via semicolons (`;`), or to put them into shell scripts and then call them.

# Example `.ini` file

(will be checked in as example.ini into this repository)

```ini
[monk]
venv=monk_venv
repo=git@github.com:DFE/MONK
path=monk
post=pip install -e .; pip install -r requirements.txt; nosetests
[monk_de]
venv=de_venv
repo=git@github.com:DFE/monk_de
path=monk_de
```

This will create two windows. One will have [MONK](/DFE/MONK) downloaded and a corresponding virtualenv created. The *post* step will install [MONK](/DFE/MONK) and it's development `requirements.txt` into the virtualenv so that nosetests can run unit tests on it. The last step of *post* starts the unit tests. So when this window is activated the first thing you'll see will be the unit test results (or the still running tests if you are a fast one). The second window will include this tool here, create a virtualenv for it and check out the code to the path `monk_de`.

You can activate it, by running the following commands:

```bash
cd path/to/example.ini
mde example
```

After that some preparation like cloning will start. In the end you will be in a tmux session. This can be recognized by that green status bar on the bottom that shows you three windows: *overview* which is not used yet but there are some ideas for later in the development, *monk*, and *monk_de*.

You can switch between the windows with `CTRL-n` (for next) and `CTRL-p` (for previous). If you want to get rid of it, for now just close all the bash sessions (`CTRL-d`) and the windows will close automatically. Of course to give it a real try, learning some [tmux](https://duckduckgo.com/?q=tmux+tutorial&t=canonical) is required.

That's it for now. Happy coding!

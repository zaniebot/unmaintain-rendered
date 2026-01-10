---
number: 12750
title: Define commands to run right after package install (via uv pip install)
type: issue
state: open
author: Miamrion
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-04-08T16:51:20Z
updated_at: 2025-12-18T17:45:52Z
url: https://github.com/astral-sh/uv/issues/12750
synced_at: 2026-01-10T01:25:24Z
---

# Define commands to run right after package install (via uv pip install)

---

_Issue opened by @Miamrion on 2025-04-08 16:51_

### Summary

Hello there, 

First of all thank you so much for the amazing work you've been doing with uv and for the amount of delivered updates. It's great ! 

We're a small animation studio which has recently adopted the use of uv as package manager mostly to handle our own custom Python libraries.
We store them on our own server, and generate virtual envs containing said libraries in order to execute software while having access to our packages. 

In the domain, there's a quite popular package manager, Rez, which offers the possibility to define one or several commands in the setup.py (they're still using this convention) of the packages. 
These commands are then automatically executed by Rez when said package is being installed into a virtual environment. 

This can be particularly practical, for example if you wish to add your packages to your PYTHONPATH automatically at install. 

So again the idea of this feature request would be : 
implement, via the package.pyproject.toml, a place to define commands which would be executed by uv at `uv pip install package`

We did not go with Rez, as uv seemed way faster and matched PEP conventions in a more graceful way ;) 
But some of the features offered by Rez could be really interesting if implemented in uv 

Here is a link to the REZ page about the commands : 
https://rez.readthedocs.io/en/3.2.1/basic_concepts.html#package-commands

Thanks a lot in advance



### Example

Let's imagine a package myLibrary
In the pyproject.toml of myLibrary, I would have something as such : 

[project]
name = "myLibrary"
version = "0.1.0"
dependencies = [
"PySide2",
"customLibrary >=0.1.0",
"anotherCustomLibrary >=0.1.0",
]
requires-python = ">=3.10"
authors = [
  {name = "Zorro", email = "zorro@magicMail.com"},
]
maintainers = [
  {name = "Zorro", email = "zorro@magicMail.com"},
]
description = "An amazing custom library made in house at MagicStudio"
[uv.commands]
sys.path.append(".") # automatically execute this part at install. Here, append package root to sys path


When using `uv pip install myLibrary` in a uv venv, uv would automatically execute the uv.commands part of the pyproject.toml file.

---

_Label `enhancement` added by @Miamrion on 2025-04-08 16:51_

---

_Comment by @zanieb on 2025-04-08 17:30_

Glad to hear you're enjoying uv :)

I think I'm broadly opposed to this — unfortunately, arbitrary command execution on install is a dangerous concept that I don't think we want to expose to all users.

Would adding `post-install` hooks to the `pyproject.toml` that executes when you install something in the _current_ project rather than defining post-install commands in arbitrary packages work for you?

---

_Label `wish` added by @zanieb on 2025-04-08 17:30_

---

_Comment by @Miamrion on 2025-04-09 07:51_

Hi zanieb, thank a lot for your fast answer ! 
I can see why you wouldn't want such a thing, outside of very specific worklows. And I feel like our need is indeed quite specific. 
I do like the idea of post-install hooks though.

Do you have an example of the usage they could have ? or the shape they could take ? 

---

_Comment by @pelson on 2025-05-07 04:02_

Author of a private tool which transparently adds post-install hooks to ``pip install``, being used by 100s of internal users for the last ~5 years, here 👋 (`.pth` files have a lot to answer for 😉). There are two reasons I have this: 

 * As I have to fix run paths of some binaries (because of reasons around the way the Python distribution was initially built)
 * As I need to gather the Java requirements of the installed Python packages to resolve Java dependencies for use with JPype (example of such a declaration https://gitlab.cern.ch/scripting-tools/pyjapc/-/blob/c47e93c6c13bd4dc95d7f0d385573d00535c1f8f/pyjapc/__init__.py#L33)

> I think I'm broadly opposed to this — unfortunately, arbitrary command execution on install is a dangerous concept that I don't think we want to expose to all users.

I totally agree that arbitrary code execution (ASE) is a scary concept. I just want to push back a little though, and state that if you install an sdist, you are exposed to ASE (both for `pyproject.toml` and `setup.py`). If you install a wheel, you are exposed to ASE on the next run of the Python interpreter.

Given the act of doing _Arbitrary Code Installation_ is the purpose of a package installer, I find it hard to see the disadvantage of explicitly declaring post-install behaviour. Perhaps it is so that you can run some checks on the installed code before you execute it? (but you must be careful not to run the interpreter before those checks are complete!).


---

_Comment by @nathanmusser on 2025-12-16 18:31_

> Would adding `post-install` hooks to the `pyproject.toml` that executes when you install something in the _current_ project rather than defining post-install commands in arbitrary packages work for you?

I'm not the original commenter but this would be really useful for my needs. A simple example is using uv to manage an infrastructure as code project using something like ansible. Being able to define some ansible-galaxy install commands as a post-install hook would be a nice QoL improvement. Currently I use small wrapper scripts that run a uv sync and then whatever other commands I need to do to finish making sure the environment is ready for use. 

---

_Comment by @konstin on 2025-12-17 16:49_

Note that there's also a security aspect to pre-/post-install scripts, which have been used recently for credential theft (https://trigger.dev/blog/shai-hulud-postmortem). This is also possible in Python using source distributions, though this would add another vector.

---

_Comment by @nathanmusser on 2025-12-18 17:45_

That's for pre-/post-install scripts defined in packages correct? 


What I was trying to express support for (but I may have misunderstood) is the addition of pre-/post-install scripts for the local project. Every time I create a new project I would need to as a user go manually defined my scripts/commands inside the project's `pyproject.toml`. I want the ability to say, "anytime this project installs packages I want to run xyz after the install is done". I agree that running arbitrary scripts defined by packages upon install is a security risk. 

---

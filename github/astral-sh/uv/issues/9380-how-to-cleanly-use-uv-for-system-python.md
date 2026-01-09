---
number: 9380
title: How To Cleanly Use UV For System Python
type: issue
state: closed
author: dapomeranz
labels: []
assignees: []
created_at: 2024-11-23T09:57:10Z
updated_at: 2024-11-23T23:48:22Z
url: https://github.com/astral-sh/uv/issues/9380
synced_at: 2026-01-07T13:12:18-06:00
---

# How To Cleanly Use UV For System Python

---

_Issue opened by @dapomeranz on 2024-11-23 09:57_

Hello,

I would like to use only `uv` to manage python installs on my computer. As I am sure we all would!

I have installed uv via brew. I have used uv python install to bring in my preferred python version. Then, I have added the following to my zshrc so that my terminal will always be able to access python.

`export PATH="$(dirname "$(uv python find)"):$PATH"`

q1. Is this the best way to set up my path? Obviously I could add parameters or static file paths to make this more specific. I was just surprised that there is no better way to just do this as a part of the uv offering? I want to make sure I am not missing anything.

Now the fun begins when I want to install global packages aka system packages in this scenario. There are many packages which I do not want to download in all of my venvs across my computer. As well as packages for miscellaneous scripting. I had to use the below command in order to get this fully setup.

`uv pip install numpy --system --break-system-packages`

The docs for --break-system-packages come with a nice warning:

```
WARNING: --break-system-packages is intended for use in continuous integration (CI) environments, when installing into Python installations that are managed by an external package manager, like apt.
It should be used with caution, as such Python installations explicitly recommend against modifications by other package managers (like uv or pip).
```

q2. Is this the best way to setup a replacement for system installed python using UV? In general, I feel that a lot of the less techincal python community would benefit from this type of setup. I would love to help others set up their python like this, but it just feels a bit wrong right now. Maybe I am missing something or maybe there is a different longer term vision still being worked on. Just thought I would write down my confusion.

Thanks to everyone who has worked so hard to make uv what it is today!

---

_Comment by @TurtleOrangina on 2024-11-23 10:26_

Hi,

I agree with you that having a "system python" provided by uv would be nice. This is a feature of `pyenv`, e.g. `pyenv global 3.10`. With `uv` I guess it is possible to use `uv run --python 3.13 -- python`, which you could alias via `alias python='uv run --python 3.13 -- python'` (e.g. in your `.zshrc`). It would be nice to have a cleaner official way of achieving this.

When it comes to your idea of installing packages into the system python, I would strongly caution you against doing so. In your numpy example, you will have some older python scripts you wrote that used numpy 1.x, and everything worked because you had numpy 1.x installed in your system. Then you work on some newer scripts and update your system python to numpy 2.x, not realizing that the old script no longer work. This can be a nightmare when you actually need to run any of your old scripts, but you have no documentation or information with which combination of of package versions this script used to work just fine! So I would always recommend to use a pyproject.toml and a uv.lock file (`uv init newproject`).

If you really don't want to use a .venv with a pyproject.toml and uv.lock file, you can use the ephermal venv that are used to run scripts directly:

```
# create your script
echo 'import numpy as np\nprint(np.array([1, 3, 3, 7]))' > test.py
# Say it needs numpy
uv add --script test.py numpy
# Now test.py has a special comment about the used python version and numpy dependency
uv run test.py # This now works, numpy is present in the ephermal venv used to run
```

This satisfies your requirement of not cluttering your disk space with many .venv that have just the same dependencies installed: The .venv is only created when you run it (emphermal venv), and it uses the normal uv caching mechanisms so it will be super fast and not need to download it every time.


---

_Comment by @notatallshaw on 2024-11-23 17:15_

I think there are some terminology issues here. I hope I can clear things up (and not make them worse):

System Python - Is typically the Python provided by your OS distro. OSes tend to have their own package manager which can install Python and Python packages. If you overwrite the OSes Python or Python packages with packages from other tools (uv, pip, poetry, hatch, pdm, etc.) you may break other things about the OS, and this is why it is **strongly discouraged** (the OS distros asked for [PEP 668](https://peps.python.org/pep-0668/) to tell tools not to install into their Python).

Global Python - This is a Python that is controlled and managed by a user tool, but is setup on the PATH so it is the default Python everywhere the user goes, it is not the system Python. Tools that allow for a global Python include conda and pyenv. This is great when you are doing non-project work or want to share an environment between projects. I believe uv has this feature in preview: https://github.com/astral-sh/uv/issues/6265#issuecomment-2461107903

Virtual Environment Python - This Python is installed in a specific directory, and is typically manually activated and not shared between projects.

I think what you want is a "Global Python", not to replace a "System Python", checkout that issue and see if works for you.


---

_Comment by @dapomeranz on 2024-11-23 23:48_

Following @notatallshaw, this all sounds correct. Looks like the global python option is what I am looking for.

---

_Closed by @dapomeranz on 2024-11-23 23:48_

---

---
number: 10300
title: "`uv pip install --system` complaints 'is externally managed'"
type: issue
state: closed
author: dadaphl
labels:
  - question
assignees: []
created_at: 2025-01-05T00:19:30Z
updated_at: 2025-02-04T02:13:58Z
url: https://github.com/astral-sh/uv/issues/10300
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip install --system` complaints 'is externally managed'

---

_Issue opened by @dadaphl on 2025-01-05 00:19_

Hello, 
When I try to install a module system wide it tells me that `The Python installation  is managed by uv`. Which is confusing and seems contradictory to me as I'm specifically using `uv` to "manage the installation" in the first place.
```
➜  ~ uv pip install --system neovim
Using Python 3.10.16 environment at: .local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu
error: The interpreter at .local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu is externally managed, and indicates the following:

  This Python installation is managed by uv and should not be modified.

Consider creating a virtual environment with `uv venv`.
```

I'm not a python dev, just relying on modules. Sorry in advance if my problem is obvious. But also by reading the doc I could not figure out if its a bug or I just don't understand how `uv pip` is intended to be used.
For context: I need to install the neovim module 'globally' for neovim have python functionality. So think that creating a virtual environment is not the right thing to do in this context.

I have installed `uv` with `yay` on `arch linux` and installed `python 3.10.16`.
```
➜  ~ python3 --version
Python 3.10.16
➜  ~ python --version
Python 3.10.16
➜  ~ which python3
/home/user/.local/bin/python3
➜  ~ which python
/home/user/.local/bin/python
➜  ~ uv -V
uv 0.5.14 (9f1ba2b96 2025-01-02)
```

---

_Comment by @zanieb on 2025-01-05 00:24_

Hi! Sorry you ran into this. We can expand that error message a bit to help clarify things

We don't allow installing into the "environment" of a managed Python installation. We require virtual environments. We only allow installing into other "system" Python environments for backwards compatibility.

As for your use-case...

> For context: I need to install the neovim module 'globally' for neovim have python functionality. So think that creating a virtual environment is not the right thing to do in this context.

How does neovim discover Python? What does the neovim module do? Can you point me to some documentation on the topic?

---

_Label `question` added by @zanieb on 2025-01-05 00:24_

---

_Comment by @dadaphl on 2025-01-05 00:51_

Wow, thank you for your very fast and helpful answer.  I apologize for my ignorance and I'm grateful you took the time to explain. Now it makes more sense.

My "neovim problem" does not seem to be specific to `uv` anymore. But I will post this never the less for others to find it.

I read deeper into the neovim doc and I "should assign one virtualenv for Nvim and hard-code the interpreter path via `python3_host_prog` so that the "pynvim" package is not required
for each virtualenv."[0]

To be honest, I'm also fuzzy about how neovim finds python. I assume its just through $PATH. Neovim offers a `:checkhealth` command which goes through a vast list of things that are `misconfiguration` if you will. 
Here is what it gives in my case. I will update when I found a solution.

```
provider.python: require("provider.python.health").check()

Python 3 provider (optional) ~
- WARNING No Python executable found that can `import neovim`. Using the first available executable for diagnostics.
- WARNING Could not load Python :
  /home/user/.local/bin/python3 does not have the "neovim" module.
  python3.12 not found in search path or not executable.
  python3.11 not found in search path or not executable.
  /home/user/.local/bin/python3.10 does not have the "neovim" module.
  python3.9 not found in search path or not executable.
  python3.8 not found in search path or not executable.
  python3.7 not found in search path or not executable.
  /home/user/.local/bin/python does not have the "neovim" module.
  - ADVICE:
    - See :help |provider-python| for more information.
    - You may disable this provider (and warning) by adding `let g:loaded_python3_provider = 0` to your init.vim
- Executable: Not found

Python virtualenv ~
- OK no $VIRTUAL_ENV
```

[0] https://neovim.io/doc/user/provider.html#python-virtualenv

---

_Comment by @dadaphl on 2025-01-05 01:02_

I forgot to answer one question
> What does the neovim module do? 

AFAIK, plugins for neovim can be written in a variety of languages (Lua, JS, Python..). The support for each language (except Lua, it's native to neovim) comes through modules that need to be installed with the respective language's package management system. 
So in my case, I'm using a plugin that seems to be (fully or partially?) written in Python. Thus I'm suppose to `pip install neovim`.
I'm also using plugins written in Js, so I had to run `npm i -g neovim`.

My "problem" is, that for some unrelated project I have to use a python virtual environment and decided to use `uv` as it seems to be the most modern approach. And now my editor doesn't work anymore :)

---

_Comment by @zanieb on 2025-01-05 01:10_

Ah I see, thanks for all the details. That makes sense.

I think what you want is something like

```
❯ uv venv ~/.local/share/neovim/venv
Using CPython 3.12.8 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtual environment at: /Users/zb/.local/share/neovim/venv
Activate with: source /Users/zb/.local/share/neovim/venv/bin/activate
❯ uv pip install neovim -p ~/.local/share/neovim/venv
Using Python 3.12.8 environment at: /Users/zb/.local/share/neovim/venv
Resolved 4 packages in 186ms
   Built neovim==0.3.1
Prepared 3 packages in 367ms
Installed 4 packages in 2ms
 + greenlet==3.1.1
 + msgpack==1.1.0
 + neovim==0.3.1
 + pynvim==0.5.2
```

Then get the Python binary path and set it as you've described, e.g.:

```
❯ echo "let g:python3_host_prog = '$(uv python find ~/.local/share/neovim/venv)'" >> init.vim
❯ cat init.vim
let g:python3_host_prog = '/Users/zb/.local/share/neovim/venv/bin/python3'
```


---

_Comment by @dadaphl on 2025-01-06 13:16_

Thank you so much. This is actually a working solution. 

---

_Closed by @dadaphl on 2025-01-06 13:16_

---

_Comment by @INFCode on 2025-01-09 05:06_

I just wanted to add more information about this "is externally managed" error. It occurs because the Python installed by `pacman` (Arch Linux's package manager) is configured to prevent modifications by tools like `pip`. This ensures that system Python is managed exclusively by `pacman`. So this error is unrelated `uv`, and will appear even when using `pip`. If you are using `pip` directly it will actually prompt you to the recommended way to install something globally: run `pacman -S python-xyz` for installing package xyz.

---

_Comment by @dadaphl on 2025-01-09 13:07_

> I just wanted to add more information about this "is externally managed" error. It occurs because the Python installed by `pacman` (Arch Linux's package manager) is configured to prevent modifications by tools like `pip`. This ensures that system Python is managed exclusively by `pacman`. So this error is unrelated `uv`, and will appear even when using `pip`. If you are using `pip` directly it will actually prompt you to the recommended way to install something globally: run `pacman -S python-xyz` for installing package xyz.

I understand that I can not install packages globally with the python installation that is managed by `pacman`. I  never intended to do so. For this I installed my global packages with pacman.

My intention was to use `uv` to install another python version that is not managed by `pacman` and install the respective packages into this "default environment" of this `uv` managed python installation.

But I think I have the wrong mental model of how python installations, environments and packages work. Like I said, I don't have experience with the python ecosystem.
I drew conclusions analogous to the node ecosystem where you install different node versions with `nvm` and each installation has its own "global space".

In my mental model it seemed like a bug that I can not use `uv` to mange a installation managed by uv. In my first post here you can see it said "This Python installation is managed by uv and should not be modified." when using a uv command.

I'm still confused. How come `pacman` still has "external managment" authority over python installation done through `uv`?

The whole "problem" originates from the fact that `uv` creates symlinks for python installations under it's management in `$USER/.local/bin` that have precedent in my `$PATH` (not sure why though). There seems to be no legal way to install global packages into this installation nor switch back to the system's python version as the default. `uv python pin` at least didn't work.

---

_Comment by @INFCode on 2025-01-10 07:35_

Oh, I think I misunderstood your case. I thought you were installing packages on the system python managed by `pacman`. So, all I said above should be unrelated. 

After reading your output again, it seems that `uv` is externally managing this python environment. It should be set by [this mechanism](https://packaging.python.org/en/latest/specifications/externally-managed-environments/#marking-an-interpreter-as-using-an-external-package-manager), which requires adding a `EXTERNALLY-MANAGED` file, so this should be intentional (though I also don't know why).

---

_Comment by @HernandoR on 2025-02-04 02:01_

> after uv python install 3.10 --default --preview when i want uv pip install jupyter-core --system it throws
uv python install 3.10 --default --preview 当我想要 uv pip install jupyter-core --system 时，它会抛出
> 
> Using Python 3.10.16 environment at: /usr/local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu
error: The interpreter at /usr/local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu is externally managed, and indicates the following:
> 
>  This Python installation is managed by uv and should not be modified.
> 
> Consider creating a virtual environment with `uv venv`.
I'm preparing a docker image, so it is intended to install Jupyter (and it's extensions) system wide
我正在准备一个 docker 镜像，因此它打算在系统范围内安装 Jupyter（及其扩展）

I think my situation is the same here. How should I write `EXTERNALLY-MANAGED`?

---

_Comment by @zanieb on 2025-02-04 02:13_

You can create a virtual environment and activate it as described in https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment

Or, you can do `--break-system-packages` to force the install.

---

_Referenced in [astral-sh/uv#15635](../../astral-sh/uv/issues/15635.md) on 2025-09-02 18:42_

---

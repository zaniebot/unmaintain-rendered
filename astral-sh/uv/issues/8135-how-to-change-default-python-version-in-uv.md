---
number: 8135
title: how to change default python version in uv? 
type: issue
state: closed
author: aizimuji
labels:
  - question
assignees: []
created_at: 2024-10-11T23:31:50Z
updated_at: 2025-04-17T12:01:25Z
url: https://github.com/astral-sh/uv/issues/8135
synced_at: 2026-01-10T01:24:24Z
---

# how to change default python version in uv? 

---

_Issue opened by @aizimuji on 2024-10-11 23:31_

for example, i just install python 3.13

if i run `uv run python` , python 3.13 version is used 

How can i change this default version to other 

maybe it shoud add a command like uv python setdeault 3.12 ?

---

_Comment by @charliermarsh on 2024-10-12 03:46_

You can add `.python-version` file to the current directory (with `3.13`) and we'll respect it! 

---

_Label `question` added by @charliermarsh on 2024-10-12 03:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-12 03:46_

---

_Comment by @leodevian on 2024-10-13 15:05_

I set the `UV_PYTHON` environment variable to `3.12` and uv now defaults to Python 3.12.

---

_Comment by @zanieb on 2024-10-13 19:33_

> You can add .python-version file to the current directory (with 3.13) and we'll respect it!

And you do this with `uv python pin 3.13`

I plan on adding the ability to change the default globally in the next breaking version — i.e. after https://github.com/astral-sh/uv/pull/6370 is merged I'll add a `--global` flag to `uv python pin`.

---

_Comment by @aizimuji on 2024-10-14 10:05_

> You can add `.python-version` file to the current directory (with `3.13`) and we'll respect it!

sometimes I need to run python interpreter for a specific version , not in a project folder 

for now I need to run uv run -p 3.12 python to run python repl in 3.12 version 

and many command need to attach -p 3.12 to specify the python version

so it's very useful to specify a global default python version

---

_Comment by @sb-travelperk on 2024-11-11 15:02_

> > You can add .python-version file to the current directory (with 3.13) and we'll respect it!
> 
> And you do this with `uv python pin 3.13`
> 
> I plan on adding the ability to change the default globally in the next breaking version — i.e. after #6370 is merged I'll add a `--global` flag to `uv python pin`.

Any update on this @zanieb?   #6370 has been merged in and I was expecting to see something new in v0.5. But the changelog doesn't contain any mention of a global flag or default version. 🤔 


---

_Comment by @kevinrenskers on 2024-11-11 20:56_

When I run `uv run python` in my home folder, it starts Python 3.11.10. Where is this default version coming from? There is no `.python-version` file in my home folder. Newer Python versions are available, so I don't understand why it picks 3.11?

---

_Comment by @glowinthedark on 2024-11-28 09:53_

### messing with `$PATH`
apparently, something like `uv python pin 3.12` is meant to be used in a venv, and **not** in a system global python context; expecting `uv` to manage global python versions will overlap with, and, possibly, mess up whatever other mechanisms your OS already has for symlinking the default version to `python3` and/or `python`.

In the simplest scenario it should be doable by adjusting your `$PATH` — (use `echo $PATH` to see your current config). Given that you control your own shell configuration  what you can do is to take note of the available pythons on your system with:

```bash
uv python list
```

let's imagine that the output you get is:

```bash
cpython-3.13.0+freethreaded-linux-aarch64-gnu    <download available>
cpython-3.12.7-linux-aarch64-gnu                 .local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/bin/python3.12
cpython-3.12.7-linux-aarch64-gnu                 .local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/bin/python3 -> python3.12
cpython-3.12.7-linux-aarch64-gnu                 .local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/bin/python -> python3.12
cpython-3.12.7-linux-aarch64-gnu                 <download available>
cpython-3.11.10-linux-aarch64-gnu                <download available>
cpython-3.11.2-linux-aarch64-gnu                 /usr/bin/python3.11
cpython-3.11.2-linux-aarch64-gnu                 /usr/bin/python3 -> python3.11
cpython-3.11.2-linux-aarch64-gnu                 /usr/bin/python -> python3
cpython-3.11.2-linux-aarch64-gnu                 /bin/python3.11
```

now edit your `~/.bashrc` or `~/.zshrc` (depending on the shell you use), and make sure that the folder with your preferred version comes **_first_** in your path, for example, for **3.12** you'd use something like `export PATH=$HOME/.local/share/uv/python/cpython-3.12.7-linux-aarch64-gnu/bin:....<OTHER/PATHS/HERE>...`

# tldr;
❗  setting system python must be a task that should normally be delegated to the OS, especially on linux systems which tend to use python for internal things, like system updates via `apt` — if you do mess with it then don't be surprised that `apt` stops working or starts throwing cryptic errors.

On debian/ubuntu-based systems the 'native' way to change the system python is with:

```bash
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 42
```

See `update-alternatives --help` for usage details.

---

_Comment by @the-citto on 2024-12-08 17:25_

I found this issue still open while searching for setting up `UV` defaults 

it would be nice to have more (most?) `CLI` settings configurable, perhaps in the `uv.toml`?

specifically for the OP issue, the default python version for `UV` to use - with the higher-precedence files or the `CLI` able to override the default(s)

#

one of the things I was hoping for was being able to automate project's initialization - searching for `requires-python` I stumbled here, but also for `ruff` 😉, `mypy`, `pyright`, and several more settings

I naively tried creating a `$XDG_CONFIG_HOME/uv/pyproject.toml` - of course no joy with that, since 

> User-and system-level configuration must use the `uv.toml` format, rather than the `pyproject.toml` format, as a `pyproject.toml` is intended to define a Python project. [1](https://docs.astral.sh/uv/configuration/files/)

I also tried `dependency-metadata` -> `requires-python` in `$XDG_CONFIG_HOME/uv/uv.toml` but no luck there either

_Note_: I just installed `UV`, tired of having to continuously having to set up the full `pyenv + git + venv + pip + pip-tools + ... + setuptools` chain all the times, so please forgive if something will come out too naive, and if something like this has already been discussed 



---

_Comment by @trim21 on 2025-02-21 11:13_

👀 so curretnly no way to set default python version for `uv tool install` ?

---

_Comment by @bavalpey on 2025-03-10 16:39_

> 👀 so curretnly no way to set default python version for `uv tool install` ?

Use the [UV_PYTHON environment variable](https://docs.astral.sh/uv/configuration/environment/#uv_python).

---

_Comment by @zanieb on 2025-03-10 16:59_

cc @jtfmumm 

---

_Comment by @dantetemplar on 2025-04-15 21:56_

@zanieb so it will be `uv python pin 3.13 --global`  in near future?

---

_Comment by @zanieb on 2025-04-15 22:06_

`--global` is already available, @jtfmumm it doesn't look like it's used for tool installs though? I have a vague recollection of discussion about that?

---

_Unassigned @charliermarsh by @zanieb on 2025-04-15 22:06_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-16 15:05_

---

_Referenced in [astral-sh/uv#12920](../../astral-sh/uv/pulls/12920.md) on 2025-04-16 15:23_

---

_Comment by @jtfmumm on 2025-04-17 12:01_

Closing in favor of #12921.

---

_Closed by @jtfmumm on 2025-04-17 12:01_

---

---
number: 1586
title: "Installing with `uv` doesn't setup cli entrypoints with pyenv"
type: issue
state: closed
author: aodj
labels:
  - compatibility
assignees: []
created_at: 2024-02-17T13:05:10Z
updated_at: 2024-06-08T14:16:01Z
url: https://github.com/astral-sh/uv/issues/1586
synced_at: 2026-01-10T01:23:07Z
---

# Installing with `uv` doesn't setup cli entrypoints with pyenv

---

_Issue opened by @aodj on 2024-02-17 13:05_

Pretty straightforward, I wanted to start using `ruff` and `uv` but when I installed `uv` then `ruff` I didn't get a `ruff` executable as expected. I ended up reinstalling it with `pip` which setup whatever entrypoint was missing.

I attempted it with another library (`pre-commit`) and got the same:

```
$ uv pip install pre-commit
...
$ pre-commit
bash: pre-commit: command not found
$ pip install pre-commit
Requirement already satisfied: pre-commit in /Users/alexander/.pyenv/versions/3.10.7/envs/venv-3.10.7/lib/python3.10/site-packages (3.6.1)
Requirement already satisfied: cfgv>=2.0.0 in /Users/alexander/.pyenv/versions/3.10.7/envs/venv-3.10.7/lib/python3.10/site-package
$ pre-commit
[WARNING] Unstaged files detected.
[INFO] Stashing unstaged files to /Users/alexander/.cache/pre-commit/patch1708173389-43374.
[INFO] Restored changes from /Users/alexander/.cache/pre-commit/patch1708173389-43374.
...
```

This is running in a virtualenv setup by pyenv, activated with a `.python-version` file (so it auto-activates when I enter the folder). I installed `uv` into the project with `pip` first.

---

_Comment by @charliermarsh on 2024-02-17 13:15_

Will take a look -- not immediately sure what's up here. Can you confirm that other executables like `black` and `ruff` work as expected?

---

_Comment by @charliermarsh on 2024-02-17 13:16_

Oh sorry, did you ensure that you activated the venv before running `pre-commit`?

---

_Comment by @aodj on 2024-02-17 14:04_

pyenv virtualenv lets you setup auto-enabling of a virtualenv by having this in your `bashrc`: `eval "$(pyenv virtualenv-init -)"` - see step 2 in the installation doc https://github.com/pyenv/pyenv-virtualenv.

Looking at the shell you get some env vars set as a result:

```
PYENV_SHELL=bash
PYENV_VIRTUALENV_INIT=1
PYENV_VIRTUAL_ENV=/Users/alexander/.pyenv/versions/3.10.7/envs/venv-3.10.7
```

---

_Comment by @aodj on 2024-02-17 14:08_

I uninstalled `ruff` to show the same

```
$ pip uninstall ruff
Found existing installation: ruff 0.2.1
Uninstalling ruff-0.2.1:
  Would remove:
    /Users/alexander/.pyenv/versions/3.10.7/envs/venv-3.10.7/bin/ruff
    /Users/alexander/.pyenv/versions/3.10.7/envs/venv-3.10.7/lib/python3.10/site-packages/ruff-0.2.1.dist-info/*
    /Users/alexander/.pyenv/versions/3.10.7/envs/venv-3.10.7/lib/python3.10/site-packages/ruff/*
Proceed (Y/n)? y
  Successfully uninstalled ruff-0.2.1
$ ruff
bash: ruff: command not found
$ pip freeze | grep -i ruff
$ uv pip install ruff
Resolved 1 package in 524ms
Installed 1 package in 19ms
 + ruff==0.2.1
$ ruff
bash: ruff: command not found
```

---

_Comment by @aodj on 2024-02-17 14:08_

If I try to activate the venv anymore, it throws an error

```
$ source activate venv-3.10.7
pyenv-virtualenv: version `venv-3.10.7' is already activated
```

---

_Comment by @aodj on 2024-02-18 22:32_

I will point out that if I follow the `uv` only usage it works, so my guess is it's some conflict with `pyenv` virtualenvs.

```
$ mkdir uv-test
$ cd uv-test
$ ls -halp
total 0
drwxr-xr-x  2 alexander  staff    64B Feb 18 22:24 ./
drwxr-xr-x  4 alexander  staff   128B Feb 18 22:27 ../
$ uv venv
Using Python 3.9.6 interpreter at /Library/Developer/CommandLineTools/usr/bin/python3
Creating virtualenv at: .venv
$ source .venv/bin/activate
(.venv) $ uv pip install ruff
Audited 1 package in 1ms
(.venv) $ ruff
Ruff: An extremely fast Python linter.

Usage: ruff [OPTIONS] <COMMAND>

Commands:
  check    Run Ruff on the given files or directories (default)
  rule     Explain a rule (or all rules)
  config   List or describe the available configuration options
  linter   List all supported upstream linters
  clean    Clear any caches in the current directory and any subdirectories
  format   Run the Ruff formatter on the given files or directories
  version  Display Ruff's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print diagnostics, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting diagnostics)

For help with a specific command, see: `ruff help <command>`.
```

---

_Comment by @aodj on 2024-02-20 19:51_

Retested with the latest 0.1.6 build and it's still not working - I was hoping the fix for #1622 might have helped but sadly not 😭 

---

_Comment by @charliermarsh on 2024-02-20 19:55_

Ah sorry, I don't think we've tried to fix this one specifically yet! Working our way through the issue tracker!

---

_Comment by @aodj on 2024-02-20 19:58_

No worries, I'm surprised no one else has encountered it (at least enough to find the thread and comment on it 😉)

---

_Comment by @mvanveen on 2024-03-11 23:32_

anecdotal and intend to investigate a little further, but I don't see CLI entry points working for `gunicorn` either.

I did try a variety of different venv configurations (both `uv venv` and `python3 -m venv`) but results were invariant.

Is this currently intended to be a supported feature?

---

_Label `bug` added by @zanieb on 2024-03-11 23:33_

---

_Comment by @zanieb on 2024-03-11 23:33_

Yes this is intended to be working (and it's worked for me previously) — we'll investigate.

---

_Comment by @zanieb on 2024-03-11 23:40_

Looks to be working as intended with a uv virtual environment:

```
❯ uv venv
Using Python 3.9.6 interpreter at: /Library/Developer/CommandLineTools/usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install gunicorn
Resolved 2 packages in 5.52s
Downloaded 2 packages in 1.13s
Installed 2 packages in 23ms
 + gunicorn==21.2.0
 + packaging==24.0
❯ ls .venv/bin
activate		activate.fish		activate_this.py	pydoc.bat		python3.9
activate.bat		activate.nu		deactivate.bat		python
activate.csh		activate.ps1		gunicorn		python3
❯ source .venv/bin/activate
❯ gunicorn --help
usage: gunicorn [OPTIONS] [APP_MODULE]
...
❯ uv version
uv 0.1.17 (ebca3197d 2024-03-11)
```

---

_Comment by @zanieb on 2024-03-11 23:41_

Is there a simple reproduction to create a `pyenv` virtual environment for standalone usage? i.e. without installing their shim and doing auto-activation and all that?

---

_Comment by @zanieb on 2024-03-11 23:47_

I did not reproduce this with `pyenv` either

```
❯ pyenv --version
pyenv 2.3.35
❯ pyenv virtualenv --version
pyenv-virtualenv 1.2.1 (python -m venv)
❯ eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
❯ pyenv virtualenv 1586
created virtual environment CPython3.7.17.final.0-64 in 181ms
  creator CPython3Posix(dest=/Users/mz/.pyenv/versions/3.7.17/envs/1586, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/Users/mz/Library/Application Support/virtualenv)
    added seed packages: pip==23.3.1, setuptools==68.0.0, wheel==0.42.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
Looking in links: /var/folders/bc/qlsk3t6x7c9fhhbvvcg68k9c0000gp/T/tmp15yhsv6z
Requirement already satisfied: setuptools in /Users/mz/.pyenv/versions/3.7.17/envs/1586/lib/python3.7/site-packages (68.0.0)
Requirement already satisfied: pip in /Users/mz/.pyenv/versions/3.7.17/envs/1586/lib/python3.7/site-packages (23.3.1)
❯ pyenv activate 1586
❯ uv pip install gunicorn
Resolved 5 packages in 11.61s
Downloaded 3 packages in 5.24s
Installed 5 packages in 34ms
 + gunicorn==21.2.0
 + importlib-metadata==6.7.0
 + packaging==24.0
 + typing-extensions==4.7.1
 + zipp==3.15.0
❯ which gunicorn
/Users/mz/.pyenv/shims/gunicorn
❯ gunicorn --version
gunicorn (version 21.2.0)
❯ ls /Users/mz/.pyenv/versions/3.7.17/envs/1586/bin
activate		activate_this.py	pip3.7			python3-config		wheel-3.7
activate.csh		gunicorn		pydoc			python3.7		wheel3
activate.fish		pip			python			python3.7-config	wheel3.7
activate.nu		pip-3.7			python-config		python3.7m-config
activate.ps1		pip3			python3			wheel
```

---

_Label `needs-mre` added by @zanieb on 2024-03-11 23:47_

---

_Comment by @aodj on 2024-03-13 15:12_

I had an MRE on my laptop at home, which I'll try to reply with later on. Given the conversation though, my gut is leaning towards something that might be related to terminal setup, so if there's any further information you require then please let me know and I'll be happy to dig into things

---

_Label `bug` removed by @charliermarsh on 2024-03-22 04:14_

---

_Comment by @akshaya-a on 2024-04-17 06:59_

~I'm running into this in github actions - using the --system flag on uv pip install doesn't enable the ruff entrypoint. switching to a venv means (i think) activating the venv on different steps which is annoying~

just kidding --system did work

---

_Comment by @ngnpope on 2024-06-07 10:35_

I'm having this problem when attempting to use `uv` in CircleCI (which uses `pyenv` internally).

Here's a simple reproducer:

```yaml
---
version: 2.1
orbs:
  python: circleci/python@2.1.1
jobs:
  reproducer:
    executor: python/default
    resource_class: small
    docker:
      - image: cimg/python:3.10
    steps:
      - run:
          name: Reproducer
          command: |
            pip install uv
            uv pip install ruff
            command ruff
          environment:
            UV_SYSTEM_PYTHON: true
workflows:
  test:
    jobs:
      - reproducer
```

For which I get the following output:

```
Collecting uv
  Downloading uv-0.2.9-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (33 kB)
Downloading uv-0.2.9-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (10.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.6/10.6 MB 164.5 MB/s eta 0:00:00

Installing collected packages: uv
Successfully installed uv-0.2.9
Resolved 1 package in 67ms
Downloaded 1 package in 113ms
Installed 1 package in 0.98ms
 + ruff==0.4.8
/bin/bash: line 3: ruff: command not found

Exited with code exit status 127
```

It seems as though no scripts get installed whatsoever as I have the same issue for `pre-commit`, `pytest`, and `ruff`.

---

_Comment by @ngnpope on 2024-06-07 10:55_

Ah, so I found [this](https://github.com/pyenv/pyenv/issues/1626#issuecomment-692118818) and sticking in a `pyenv rehash` before `command ruff` in my example above fixes this. It's all because the shim executed for `pip` by `pyenv` will automatically do this, but there isn't something to do that for `uv` automatically. It looks like one could be added [here](https://github.com/pyenv/pyenv/tree/master/pyenv.d/exec/pip-rehash) if someone wants to work it out.

---

_Comment by @zanieb on 2024-06-07 13:41_

Oh interesting, thanks for digging into it @ngnpope!

---

_Referenced in [pyenv/pyenv#2979](../../pyenv/pyenv/issues/2979.md) on 2024-06-07 13:46_

---

_Referenced in [astral-sh/uv#4130](../../astral-sh/uv/issues/4130.md) on 2024-06-07 13:47_

---

_Comment by @zanieb on 2024-06-07 13:47_

I've opened a couple issues at https://github.com/astral-sh/uv/issues/4130 and https://github.com/pyenv/pyenv/issues/2979 to track this. 

---

_Label `needs-mre` removed by @zanieb on 2024-06-07 13:48_

---

_Label `compatibility` added by @zanieb on 2024-06-07 13:48_

---

_Comment by @zanieb on 2024-06-08 14:15_

I think I'm going to close this one since we do correctly setup CLI entry points. Note the workaround here is to run `pyenv rehash` for _them_ to update their shims to respect the new entry points. We'll track "automatic" rehash support in #4130.

---

_Closed by @zanieb on 2024-06-08 14:15_

---

_Renamed from "Installing with `uv` doesn't setup cli entrypoints" to "Installing with `uv` doesn't setup cli entrypoints with pyenv" by @zanieb on 2024-06-08 14:16_

---

_Referenced in [astral-sh/uv#6675](../../astral-sh/uv/issues/6675.md) on 2024-08-27 23:08_

---

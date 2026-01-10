```yaml
number: 1674
title: "ui-consistency:  should subcommands  with no argument show help by default?"
type: issue
state: closed
author: woutervh
labels:
  - question
  - cli
assignees: []
created_at: 2024-02-19T01:24:23Z
updated_at: 2024-06-19T00:35:15Z
url: https://github.com/astral-sh/uv/issues/1674
synced_at: 2026-01-10T05:31:36Z
```

# ui-consistency:  should subcommands  with no argument show help by default?

---

_Issue opened by @woutervh on 2024-02-19 01:24_

Was it a deliberate decicsion to make `uv venv`  directly execute?
This is rather unexpected behaviour.

By default, usually subcommands show help:


```
> uv pip
Resolve and install Python packages

Usage: uv pip [OPTIONS] <COMMAND>

Commands:
  compile    Compile a `requirements.in` file to a `requirements.txt` file
  sync       Sync dependencies from a `requirements.txt` file
  install    Install packages into the current environment
  uninstall  Uninstall packages from the current environment
  freeze     Enumerate the installed packages in the current environment
  help       Print this message or the help of the given subcommand(s)

Options:
  -q, --quiet                  Do not print any output
  -v, --verbose                Use verbose output
      --color <COLOR>          Control colors in output [default: auto] [possible values: auto, always, never]
  -n, --no-cache               Avoid reading from or writing to the cache [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]
  -h, --help                   Print help (see more with '--help')
  -V, --version                Print version

```

But venv does not:
```
> uv venv
uv venv
Using Python 3.11.6 interpreter at /opt/tools/pyenv/var/repo/versions/3.11.6/bin/python3
Creating virtualenv at: .venv
```

```
> uv venv --help
Create a virtual environment

Usage: uv venv [OPTIONS] [NAME]
...
```

I find it rather unexpected to not have to provide a path:
( I accidentally created a .venv twice because I wanted to see the options)

coming from virtualenv, I would have expected:
```
> uv venv .
> uv venv .venv
```


Executing virtualenv directly:
```
> virtualenv
usage: virtualenv [--version] [--with-traceback] [-v | -q] [--read-only-app-data] [--app-data APP_DATA] [--reset-app-data] [--upgrade-embed-wheels] [--discovery {builtin}] [-p py] [--try-first-with py_exe]
                  [--creator {builtin,cpython3-posix,venv}] [--seeder {app-data,pip}] [--no-seed] [--activators comma_sep_list] [--clear] [--no-vcs-ignore] [--system-site-packages] [--symlinks | --copies] [--no-download | --download]
                  [--extra-search-dir d [d ...]] [--pip version] [--setuptools version] [--wheel version] [--no-pip] [--no-setuptools] [--no-wheel] [--no-periodic-update] [--symlink-app-data] [--prompt prompt] [-h]
                  dest
virtualenv: error: the following arguments are required: dest
SystemExit: 2
```


Also for ruff:  executing `ruff` is the same as `ruff --help`




---

_Label `question` added by @zanieb on 2024-02-19 02:01_

---

_Label `cli` added by @zanieb on 2024-02-19 02:01_

---

_Comment by @zanieb on 2024-02-19 03:49_

I think this is intentional so you can create a virtual environment without specifying a name. I'm not sure every subcommand should be required to take an argument.

---

_Closed by @zanieb on 2024-06-19 00:35_

---

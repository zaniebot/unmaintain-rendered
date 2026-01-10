---
number: 12877
title: no-config and config-file arguments still read pyproject.toml
type: issue
state: open
author: g4borg
labels:
  - question
assignees: []
created_at: 2025-04-14T09:31:09Z
updated_at: 2025-05-15T21:05:08Z
url: https://github.com/astral-sh/uv/issues/12877
synced_at: 2026-01-10T01:25:26Z
---

# no-config and config-file arguments still read pyproject.toml

---

_Issue opened by @g4borg on 2025-04-14 09:31_

### Summary

I came across an issue when trying to use VS Code internal terminal, which starts up in the project directory, when using uv to automate some of my things in bashrc

Basically, it was reading the pyproject.toml of that project, which had no project entry, and returned with an error, even if the uv call was designed to run for a different virtual env.

I boiled it down to trying in the same project directory, trying all kinds of flags to make it ignore the pyproject in the current directory, with different combinations of using --no-config and --config-file:

```
❯ UV_NO_CONFIG=1 uv --no-config --config-file="/home/g4b/.local/python/uv.toml" run python

error: No `project` table found in: `/home/g4b/projects/redacted/pyproject.toml
```

Expected according to the docs:
 --no-config should ignore the pyproject.toml
 --config-file should read that file and ignore pyproject.toml as well

Instead:
  error message and uv exiting further instructions

Yes I did a workaround by adding "cd ~/.local/python && uv run ..." into my script, which works for me, but I still think this might be a bug, that also should be easy to replicate by just running such commands in a directory with an empty pyproject.toml file.

### Platform

Windows, Ubuntu 22.04 WSL

### Version

uv 0.6.14

### Python version

any

---

_Label `bug` added by @g4borg on 2025-04-14 09:31_

---

_Comment by @konstin on 2025-04-14 09:49_

Are you looking for `--no-project`? `--no-config`/`--config-file` are for uv's configuration (things that can go into `uv.toml`), while in the example it fails at the project metadata.

---

_Comment by @g4borg on 2025-04-14 13:33_

that indeed works (run --no-project), but should --no-config not as advertised skip reading config files in the current project directory?

Pardon me if I am confused

---

_Label `question` added by @Gankra on 2025-04-14 17:01_

---

_Label `bug` removed by @Gankra on 2025-04-14 17:01_

---

_Comment by @TrevorBenson on 2025-05-15 21:05_

The add subcommand does not have a `--no-project` option and results in trying to find config files:
```
user@host:~/testing/uv-no-config$ uv venv
Using CPython 3.13.3 interpreter at: /usr/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
user@host:~/testing/uv-no-config$ source .venv/bin/activate
(uv-no-config) user@host:~/testing/uv-no-config$ uv add --no-config --active fastapi
error: No `pyproject.toml` found in current directory or any parent directory
```

However, if I init a project, create a subdir, add a .venv to the subdir, and then try again using `--no-config` it clearly discovers and parses the `pyproject.toml` configuration file, as it has a presumed path for the .venv:
```
user@host:~/testing/uv-no-config$ uv init
Initialized project `uv-no-config`
user@host:~/testing/uv-no-config$ mkdir subdir
user@host:~/testing/uv-no-config$ cd subdir
user@host:~/testing/uv-no-config/subdir$ uv venv
Using CPython 3.13.3 interpreter at: /usr/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
user@host:~/testing/uv-no-config/subdir$ source .venv/bin/activate
(subdir) user@host:~/testing/uv-no-config/subdir$ uv add --no-config fastapi
warning: `VIRTUAL_ENV=.venv` does not match the project environment path `/home/user/testing/uv-no-config/.venv` and will be ignored; use `--active` to target the active environment instead
Using CPython 3.13.2
Creating virtual environment at: /home/user/testing/uv-no-config/.venv
Resolved 11 packages in 245ms
Installed 10 packages in 9ms
 + annotated-types==0.7.0
 + anyio==4.9.0
 + fastapi==0.115.12
 + idna==3.10
 + pydantic==2.11.4
 + pydantic-core==2.33.2
 + sniffio==1.3.1
 + starlette==0.46.2
 + typing-extensions==4.13.2
 + typing-inspection==0.4.0
```

Using `--active` with `--no-config` does work, but **only** when there is a project in a parent of the path.

I can open a feature request for `--no-project` for the add subcommand, but even then the current usage menu suggesting `--no-config` blocks discovery of `pyproject.toml` seems inaccurate, maybe it should be updated if its behavior is not considered to be a bug. 

---

```yaml
number: 7519
title: "`uv sync` deletes data in non-virtualenv `UV_PROJECT_ENVIRONMENT` without warning"
type: issue
state: closed
author: jodal
labels:
  - bug
assignees: []
created_at: 2024-09-18T21:00:07Z
updated_at: 2024-09-19T12:08:48Z
url: https://github.com/astral-sh/uv/issues/7519
synced_at: 2026-01-12T15:59:14Z
```

# `uv sync` deletes data in non-virtualenv `UV_PROJECT_ENVIRONMENT` without warning

---

_@jodal_

### Environment

```sh
❯ uv --version
uv 0.4.12
```

### Background

I have a directory `projects` containing multiple related Git repos (`bar`, `baz`, etc) with Python projects that all need to be installed into the same virtualenv because they are a core and a set of extensions.

At https://docs.astral.sh/uv/configuration/environment/ I read about `UV_PROJECT_ENVIRONMENT`:

> Use to specify the path to the directory to use for a project virtual environment. See the [project documentation](https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path) for more details.

At this point, it was a bit unclear to me if this variable was:

1. supposed to point to the virtualenv directory itself, or
2. supposed to point to a directory that would contain the `.venv` directory.

I learned the hard way that 1 is the right interpretation, but I went with 2 without testing my assumptions.

### The mishap

So, given this setup:

```sh
❯ mkdir projects
❯ cd projects 
~/projects ❯ uv init bar
Initialized project `bar` at `/home/jodal/projects/bar`
~/projects ❯ uv init baz
Initialized project `baz` at `/home/jodal/projects/baz`
~/projects ❯ touch valuable-data.txt
~/projects ❯ ls -l
total 8
drwxrwxr-x 2 jodal jodal 4096 sep.  18 22:45 bar
drwxrwxr-x 2 jodal jodal 4096 sep.  18 22:45 baz
-rw-rw-r-- 1 jodal jodal    0 sep.  18 22:45 valuable-data.txt
```

I assumed that `UV_PROJECT_ENVIRONMENT` was supposed to point to the directory containing the `.venv`, without testing my assumption, and did the following:

```sh
~/projects ❯ cd bar 
~/projects/bar ❯ UV_PROJECT_ENVIRONMENT=.. uv sync    
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: /home/jodal/projects/bin/python3
Using Python 3.12.6 interpreter at: /home/jodal/.local/share/mise/installs/python/3.12/bin/python3.12
Creating virtualenv at: ..
error: Failed to build: `bar @ file:///home/jodal/projects/bar`
  Caused by: /home/jodal/projects/bar does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

At this point, the `projects` directory is a virtualenv. The `bar` and `baz` projects are gone, as well as the `valuable-data.txt` file.

```sh
❯ ls -l ~/projects 
total 20
drwxrwxr-x 2 jodal jodal 4096 sep.  18 22:45 bar
drwxrwxr-x 2 jodal jodal 4096 sep.  18 22:45 bin
-rw-rw-r-- 1 jodal jodal   43 sep.  18 22:45 CACHEDIR.TAG
drwxrwxr-x 3 jodal jodal 4096 sep.  18 22:45 lib
lrwxrwxrwx 1 jodal jodal    3 sep.  18 22:45 lib64 -> lib
-rw-rw-r-- 1 jodal jodal  173 sep.  18 22:45 pyvenv.cfg
❯ ls -l ~/projects/bar              
total 0
❯ ls -l ~/projects/baz
ls: cannot access '/home/jodal/projects/baz': No such file or directory
❯ ls -l ~/projects/valuable-data.txt
ls: cannot access '/home/jodal/projects/valuable-data.txt': No such file or directory
```

I realize that this was my fault for not testing assumptions, and no data not tracked in Git was lost. However, the behavior was surprising.

### Suggestions

#### Documentation

Make the documentation for `UV_PROJECT_ENVIRONMENT` even more explicit about it pointing to the virtualenv directory itself.

#### Behavior

Only let `uv sync` create/recreate a virtualenv if the path pointed to by `UV_PROJECT_ENVIRONMENT` either

1. does not exist,
2. is an empty directory, or
3. is already a virtualenv.

In any other case, abort with a message explaining the problem.

---

_Comment by @zanieb on 2024-09-18 21:59_

Yeah this seems like an oversight.

---

_Label `bug` added by @zanieb on 2024-09-18 21:59_

---

_Comment by @zanieb on 2024-09-18 22:44_

(If someone wants to look into this feel free, otherwise I'll try to do so soon)

---

_Closed by @zanieb on 2024-09-19 11:21_

---

_Closed by @zanieb on 2024-09-19 11:21_

---

_Comment by @jodal on 2024-09-19 12:08_

Thanks! ❤️

---

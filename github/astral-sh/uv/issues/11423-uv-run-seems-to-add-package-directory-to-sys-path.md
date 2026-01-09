---
number: 11423
title: "`uv run` seems to add package directory to `sys.path`"
type: issue
state: closed
author: NiklasRosenstein
labels:
  - bug
assignees: []
created_at: 2025-02-11T15:50:29Z
updated_at: 2025-02-14T11:12:23Z
url: https://github.com/astral-sh/uv/issues/11423
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv run` seems to add package directory to `sys.path`

---

_Issue opened by @NiklasRosenstein on 2025-02-11 15:50_

### Question

At first I thought this must be easily reproducible, but now I'm actually a bit stuck and unsure if this is really an issue in UV. But I can't seem to figure out how it could _not_ be UV, either.

The issue I'm facing is that I'd like to add a `pytr.types` module, but when using `uv run pytr` it fails because `pytr.types` is also importable as `types`. Adding to `pytr/__init__.py`:

```diff
+import sys
+print("imported pytr.__init__; sys.path =", sys.path)
```

and running `uv run pytr`, I get:

```console
$ uv run pytr
imported pytr.__init__; sys.path = ['/home/niklas/git/pytr/pytr', '/home/niklas/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/python313.zip', '/home/niklas/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/python3.13', '/home/niklas/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/python3.13/lib-dynload', '/home/niklas/git/pytr/.venv/lib/python3.13/site-packages', '/home/niklas/git/pytr']
```

However, when run not _directly_ with Uv:

```console
$ uv run bash -c pytr
imported pytr.__init__; sys.path = ['/home/niklas/git/pytr/.venv/bin', '/home/niklas/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/python313.zip', '/home/niklas/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/python3.13', '/home/niklas/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/lib/python3.13/lib-dynload', '/home/niklas/git/pytr/.venv/lib/python3.13/site-packages', '/home/niklas/git/pytr']
```

Note how the first contains the path `/home/niklas/git/pytr/pytr`, but it shouldn't. Where does this path come from, and is this a bug in UV or somewhere in the project?

You can reproduce this on the latest version of https://github.com/pytr-org/pytr ([f830c55](https://github.com/pytr-org/pytr/commit/f830c55a59f5d8233a06e7328184886601583ff5)).


### Platform

WSL2 Ubuntu 22.04

### Version

0.5.30

---

_Label `question` added by @NiklasRosenstein on 2025-02-11 15:50_

---

_Comment by @zanieb on 2025-02-11 16:25_

I did a quick demo of this and your assessment seems right?

```
rm -rf example
mkdir example
cd example
mkdir example_module
touch example_module/__init__.py
echo "import sys; print(sys.path)" > example_module/__main__.py
```

```
❯ uv run example_module
['/Users/zb/workspace/uv/example/example_module', ...]

❯ python -m example_module
['/Users/zb/workspace/uv/example', ...]
```

I'm a little confused by that, since my understanding is we're just invoking `python -m`. Can investigate soon...

---

_Label `question` removed by @zanieb on 2025-02-11 16:25_

---

_Label `bug` added by @zanieb on 2025-02-11 16:25_

---

_Assigned to @zanieb by @zanieb on 2025-02-11 16:29_

---

_Comment by @NiklasRosenstein on 2025-02-11 17:15_

I did not realize `uv run` works with just a module name. `:surprised-pikachu-face:`

Though I think that gives us a clue as to what's going on, and explains why I could at first not reproduce this in a smaller example. If you add a `pyproject.toml` with

```toml
[project.scripts]
example-module = "example_module.__main__:main"
```

You will see the difference:

```console
$ uv run example_module
['/home/niklas/Desktop/example-module/example_module', ...]
$ uv run example-module
['/home/niklas/Desktop/example-module/.venv/bin', ...]
```

Looks to me like it's going wrong where Uv decides to run the module over the entry point, which also seems like another bug of its own right. `uv run pytr` should probably prefer to use what's defined in the entrypoints rather than executing the module's `__main__`.

---

_Comment by @zanieb on 2025-02-11 21:27_

Agree. I thought we fixed that but perhaps not... trying to find the previous discussion still.

---

_Comment by @zanieb on 2025-02-11 21:30_

Aha https://github.com/astral-sh/uv/issues/9167

---

_Referenced in [astral-sh/uv#11431](../../astral-sh/uv/pulls/11431.md) on 2025-02-11 22:10_

---

_Closed by @zanieb on 2025-02-12 18:08_

---

_Closed by @zanieb on 2025-02-12 18:08_

---

_Comment by @NiklasRosenstein on 2025-02-13 07:45_

Thanks for looking into this, @zanieb!

Checking out Uv 0.5.31 right now, I've got a few follow up questions.

- If `uv run <name>` should run a module that matches that name (when no other entrypoint or binary in `PATH` exists (?)), does that not mean it should run that module whether it lives in the current working directory or not (I.e. whatever is importable)?

  That does not seem to be the case right now (although, to be fair, it seems this behaviour has not changed from 0.5.30):

  ```console
  ❯ tree
  .
  ├── pyproject.toml
  ├── README.md
  ├── src
  │   └── example_module
  │       ├── __init__.py
  │       └── __main__.py
  └── uv.lock
  
  ❯ uv run example_module
  error: Failed to spawn: `example_module`
    Caused by: No such file or directory (os error 

  ❯ mv src/example_module/ .

  ❯ uv run example_module
  Hello!
  ```

- I think that the package path in `sys.path` is still unexpected and should not be there. When you have a Python package, you don't expect (and don't want) to be able to import it's submodules as top-level modules.
  
  ```console
  ❯ echo "import sys; print(sys.path)" > example_module/__main__.py
  
  ❯ uv run example_module
  ['/home/niklas/Desktop/example-module/example_module',  ...]
  ```

(All uv commands show with version 0.5.31)

---

_Comment by @zanieb on 2025-02-13 15:19_

> If uv run <name> should run a module that matches that name (when no other entrypoint or binary in PATH exists (?)), does that not mean it should run that module whether it lives in the current working directory or not (I.e. whatever is importable)?

It's actually emulating the `python <name>` functionality

```
❯ mkdir example
❯ cd example
❯ mkdir foo
❯ touch foo/__init__.py
❯ touch foo/__main__.py
❯ python foo
```

This requires the module to be in the working directory

And the path is the same for `python` and `uv run`

```
❯ python foo
['/Users/zb/workspace/uv/example/foo', ...]
❯ uv run foo
['/Users/zb/workspace/uv/example/foo', ...]
```

I know this is a little confusing, but we just implemented the `python <name>` parity.

>  (when no other entrypoint or binary in PATH exists (?)),

We don't read the `PATH`, just the virtual environment bin. So the Python module could take precedence in some cases still.

---

_Comment by @NiklasRosenstein on 2025-02-14 11:12_

TIL `python <path-to-directory>` is a thing. Thanks 👍 

---

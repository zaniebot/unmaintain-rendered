---
number: 2144
title: "`sys.version_info` not being picked up from virtual environment"
type: issue
state: closed
author: dedeswim
labels: []
assignees: []
created_at: 2025-12-21T12:05:19Z
updated_at: 2025-12-22T12:06:47Z
url: https://github.com/astral-sh/ty/issues/2144
synced_at: 2026-01-10T01:52:52Z
---

# `sys.version_info` not being picked up from virtual environment

---

_Issue opened by @dedeswim on 2025-12-21 12:05_

### Summary

Hi, thank you so much for ty!

In my codebase I have this:

```py
if sys.version_info < (3, 11):
    from exceptiongroup import ExceptionGroup
```

My environment looks like this:

- It is managed by `uv`, it is in `.venv` and it does not have any special configuration
- The error is shown even if I run `uv run ty check src --python .venv/bin/python`
- The error is not shown if I run `uv run ty check src --python-version 3.13`
- I use uv to manage the project, and `.python-version` is 3.13

And in `pyproject.toml` I have

```toml
dependencies = [
    # Only needed for py3.10 as >=3.11 comes with
    # ExceptionGroup in the standard library
    "exceptiongroup>=1.2.0; python_version < '3.11'",
]
```

However, when I run `uv run ty check src`, I get

```
uv run ty check src/prompt_siren/run.py
error[unresolved-import]: Cannot resolve imported module `exceptiongroup`
  --> src/prompt_siren/run.py:14:10
   |
12 | # ExceptionGroup is built-in in Python 3.11+, needs backport for 3.10
13 | if sys.version_info < (3, 11):
14 |     from exceptiongroup import ExceptionGroup
   |          ^^^^^^^^^^^^^^
15 |
16 | import anyio
   |
info: Searched in the following paths during module resolution:
info:   1. /Users/edoardo/Documents/research/projects/prompt-siren/src (first-party code)
info:   2. /Users/edoardo/Documents/research/projects/prompt-siren (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /Users/edoardo/Documents/research/projects/prompt-siren/.venv/lib/python3.13/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```

You can find a minimal example to reproduce this issue [here](https://github.com/dedeswim/ty-py-version-issue).

I know I can specify `python-version = "3.13"` in `pyproject.toml` but I think that ideally `ty` should pick the Python version from the virtual environment.

My issue looks similar to #1733, but I don't think it's exactly the same, as in my case all other packages are correctly recognized and imported, and this one is specifically related to picking the right python-version from the environment.

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Comment by @AlexWaygood on 2025-12-21 12:49_

Hey, thanks for the report! I'm guessing you have a low `requires-python` set in your pyproject.toml file?

I think this issue is the same as https://github.com/astral-sh/ty/issues/1708

---

_Comment by @AlexWaygood on 2025-12-21 12:51_

> I think that ideally `ty` should pick the Python version from the virtual environment

We do have have this fallback already, but it only triggers if you don't have a `requires-python` set in your pyproject.toml file. If we see that setting there, we assume you probably want your project to type-check with the lowest version of Python your project supports, so we assume that version when type-checking your project

---

_Comment by @AlexWaygood on 2025-12-21 16:22_

We don't read `.python-version` files at all right now, but even if we did I think we'd probably only fallback to them if your pyproject.toml file _didn't_ have a `requires-python` configuration field set.

---

_Comment by @dedeswim on 2025-12-22 08:46_

Thank you so much for the reply!

> I'm guessing you have a low requires-python set in your pyproject.toml file?

Yeah, it is indeed `requires-python = ">=3.10,<3.14"`

> but it only triggers if you don't have a requires-python set in your pyproject.toml file.

So how would you recommend approaching cases where you need certain dependencies only if using a given Python version? To use `--python-version`? To set `.python-version` to the lowest supported version so that `uv` uses that one?

> We don't read .python-version files at all right now

Yeah I do not expect ty to read `.python-version`. I just mentioned this to say that `uv` is set up to use Python 3.13.

Btw, I believe that pyright and basedpyright prioritize the venv version over the lowest supported `requires-python` in pyproject.toml

Also, yeah it seems to be a duplicate of #1708. Sorry about that! We can discuss there!



---

_Comment by @AlexWaygood on 2025-12-22 12:06_

> So how would you recommend approaching cases where you need certain dependencies only if using a given Python version? To use `--python-version`? To set `.python-version` to the lowest supported version so that `uv` uses that one?

I would include `exceptiongroup` as a dev-dependency on all Python versions, even though it's only a runtime dependency for you on Python <3.11, so that you can easily type-check your code locally with a virtual environment that uses a newer version of Python

> Also, yeah it seems to be a duplicate of https://github.com/astral-sh/ty/issues/1708. Sorry about that!

Oh, no problem at all! Finding dupes on GitHub is ~impossible, especially on a project full of jargon like a type checker 😃

---

_Closed by @AlexWaygood on 2025-12-22 12:06_

---

```yaml
number: 171
title: Discovery of local venv
type: issue
state: closed
author: MichaReiser
labels:
  - imports
assignees: []
created_at: 2025-03-14T10:42:34Z
updated_at: 2025-05-12T20:37:22Z
url: https://github.com/astral-sh/ty/issues/171
synced_at: 2026-01-12T15:54:22Z
```

# Discovery of local venv

---

_@MichaReiser_

Red Knot currently requires users to configure the path to their venv explicitly. Red Knot should try to detect the presence of a venv by following community best practices (`$VIRTUAL_ENV`, `.venv`, `venv`). 

Related: [uv's supported python sources](https://github.com/astral-sh/uv/blob/6cc7a560f72a9e34907df5309d61980dac52a044/crates/uv-python/src/discovery.rs#L181-L202)

---

_Comment by @AlexWaygood on 2025-03-14 19:21_

> Red Knot should try to detect the precense of a venv by following community best practices (`$VIRTUAL_ENV`, `.venv`, `venv`).

The last two of these are sort-of heuristics: virtual environments are also often called `.env`, `env`, or something else altogether. I'm okay with `.venv`, as there have been effors (supported by uv and several other tools) to make `.venv` a standard name that should be used by all Python virtual environments. But I'd vote for keeping the special-casing to just `.venv`, and not applying the special-casing to `venv`. It's a much less common name for the virtual environment, and there's no attempt to make a standard around it.

---

_Renamed from "[red-knot] Disocvery of local venv" to "[red-knot] Discovery of local venv" by @AlexWaygood on 2025-03-14 20:41_

---

_Closed by @AlexWaygood on 2025-03-20 13:55_

---

_Closed by @AlexWaygood on 2025-03-20 13:55_

---

_Reopened by @MichaReiser on 2025-03-20 13:56_

---

_Comment by @MichaReiser on 2025-03-20 13:56_

Let's keep this open because we may want to support picking up `.venv` even if `VIRTUAL_ENV` isn't set.

---

_Comment by @AlexWaygood on 2025-03-20 13:57_

> Let's keep this open because we may want to support picking up `.venv` even if `VIRTUAL_ENV` isn't set.

If we do this, do we want to also support a `--no-site-packages` flag to allow users to avoid this implicit fallback? It might not be welcome in all cases; there are some cases where you want to ensure isolation and don't want the type checker to use any types from the virtual environment, even if there is one

---

_Comment by @MichaReiser on 2025-03-21 12:46_

One more thing we should consider here is the LSP integration where `VIRTUAL_ENV` is unlikely to be set. 

---

_Comment by @AlexWaygood on 2025-03-21 12:50_

Huh, I always have the project's virtual environment activated when I'm working on a Python project in my IDE (and if the virtual environment is activated, the `VIRTUAL_ENV` variable is certain to be set). It's impossible to e.g. run tests from a terminal in my IDE if the correct virtual environment isn't activated.

But even if the virtual environment isn't activated, pylance always forces me to explicitly tell it which system installation or virtual environment it should use for the project (from a drop-down menu) before I can start working on a project

---

_Comment by @MichaReiser on 2025-03-21 13:25_

Yes, this is a functionality that pylance (the python extension) provides. We should either integrate with the python extension (the downside here is that the python extension comes with its own LSP) or implement our own Python/venv discovery.



---

_Closed by @MichaReiser on 2025-03-28 17:59_

---

_Closed by @MichaReiser on 2025-03-28 17:59_

---

_Reopened by @MichaReiser on 2025-03-28 18:00_

---

_Renamed from "[red-knot] Discovery of local venv" to "Discovery of local venv" by @MichaReiser on 2025-05-07 15:26_

---

_Comment by @collinanderson on 2025-05-11 02:30_

Not sure if this is related or not, but `uv run --with=ty ty check` fails to pick up the rest of my `.venv`.

---

_Label `imports` added by @AlexWaygood on 2025-05-11 08:02_

---

_Comment by @zanieb on 2025-05-11 15:52_

We might not support nested virtual environments like uv uses for `--with` in ty yet.

---

_Comment by @AlexWaygood on 2025-05-11 15:54_

> We might not support nested virtual environments like uv uses for `--with` in ty yet.

What does uv set the `VIRTUAL_ENV` variable to if it's running a command with one of these nested virtual environments?

---

_Comment by @zanieb on 2025-05-11 15:57_

It would have to be the nested one, so those packages are available.

---

_Comment by @zanieb on 2025-05-11 15:59_

See https://github.com/astral-sh/uv/blob/f557ea382378dba716471b3021611abe8b0a3bbe/crates/uv/src/commands/project/run.rs#L980-L1006

---

_Comment by @AlexWaygood on 2025-05-11 16:01_

Huh, we should support these nested venvs in ty already then.

At a guess: maybe some of the packages @collinanderson is importing in his repo are only listed as dev-dependencies, and he hasn't passed `--dev`, so uv hasn't included them in the environment? Hard to tell without more specifics, though

---

_Comment by @collinanderson on 2025-05-11 16:52_

Steps to reproduce (CPython 3.13.2, uv 0.7.3, ty 0.0.0-alpha.8):

```
uv init tmp
cd tmp
uv add httpx
echo 'import httpx' >> main.py
uv run --with ty ty check
```
ty says:
```
error[unresolved-import]: Cannot resolve imported module `httpx`
```
it works fine if you `uv add ty`, but doesn't work when you `uv run --with ty ty check`. `uv run --with mypy mypy .` works fine and finds the import.

<details>
<summary>uv run --with ty ty check --verbose --verbose</summary>

```
2025-05-11 12:51:51.281544701 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-05-11 12:51:51.281577281 DEBUG Version: 0.0.0-alpha.8
2025-05-11 12:51:51.281602471 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-05-11 12:51:51.281703641 DEBUG Searching for a project in '/home/collin/tmp'
2025-05-11 12:51:51.281806121 DEBUG Resolving requires-python constraint: `>=3.13`
2025-05-11 12:51:51.281832661 DEBUG Resolved requires-python constraint to: 3.13
2025-05-11 12:51:51.281854762 DEBUG Project without `tool.ty` section: '/home/collin/tmp'
2025-05-11 12:51:51.281868452 DEBUG Searching for a user-level configuration at `/home/collin/.config/ty/ty.toml`
2025-05-11 12:51:51.281893422 INFO Defaulting to python-platform `linux`
2025-05-11 12:51:51.281917742 INFO Python version: Python 3.13, platform: linux
2025-05-11 12:51:51.281930492 DEBUG Adding first-party search path '/home/collin/tmp'
2025-05-11 12:51:51.281947742 DEBUG Using vendored stdlib
2025-05-11 12:51:51.282668282 DEBUG Discovering site-packages paths from sys-prefix `/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY` (`VIRTUAL_ENV` environment variable')
2025-05-11 12:51:51.282701342 DEBUG Attempting to parse virtual environment metadata at '/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY/pyvenv.cfg'
2025-05-11 12:51:51.282745112 DEBUG Searching for site-packages directory in `sys.prefix` path `/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY`
2025-05-11 12:51:51.282753712 DEBUG Resolved site-packages directories for this virtual environment are: ["/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY/lib/python3.13/site-packages"]
2025-05-11 12:51:51.282758592 DEBUG Adding site-packages search path '/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY/lib/python3.13/site-packages'
2025-05-11 12:51:51.282942792 DEBUG Starting main loop
Checking ------------------------------------------------------------ 0/0 files                                                                                                    2025-05-11 12:51:51.283013262 DEBUG Waiting for next main loop message.
2025-05-11 12:51:51.283144902 DEBUG Checking project 'tmp'
2025-05-11 12:51:51.285666824 INFO Indexed 1 file(s)
Checking ------------------------------------------------------------ 0/1 files                                                                                                    2025-05-11 12:51:51.286172246 DEBUG Checking file '/home/collin/tmp/main.py'
2025-05-11 12:51:51.316921754 DEBUG Resolving dynamic module resolution paths
2025-05-11 12:51:51.317122364 DEBUG Module `httpx` not found in search paths
error[unresolved-import]: Cannot resolve imported module `httpx`
```
</details>

---

_Comment by @zanieb on 2025-05-11 17:00_

Let's track this separately from here — https://github.com/astral-sh/ty/issues/320

---

_Comment by @AlexWaygood on 2025-05-12 20:37_

I'm not sure there's anything left to do here. Everything in the issue's original writeup has been implemented, and so has https://github.com/astral-sh/ty/issues/171#issuecomment-2859026083. If there's anything more to be done in this area, I think it would be easier to track if we opened specific issues for the followups

---

_Closed by @AlexWaygood on 2025-05-12 20:37_

---

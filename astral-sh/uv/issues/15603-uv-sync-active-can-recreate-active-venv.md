---
number: 15603
title: " `uv sync --active` can recreate active venv"
type: issue
state: open
author: antonysouthworth-halter
labels:
  - bug
  - breaking
assignees: []
created_at: 2025-08-31T06:07:10Z
updated_at: 2025-10-13T00:28:30Z
url: https://github.com/astral-sh/uv/issues/15603
synced_at: 2026-01-10T01:25:57Z
---

#  `uv sync --active` can recreate active venv

---

_Issue opened by @antonysouthworth-halter on 2025-08-31 06:07_

This is my usual workflow for setting up a Python project:

```
$ asdf set python 3.12.11 # or, whatever python version I want to use
$ python -m venv .venv
$ . .venv/bin/activate
$ python -m pip install --upgrade pip; python -m pip install --upgrade uv
$ uv sync
```

BUT it seems maybe some recent change to `uv` breaks this workflow as this is the output I get:

```
$ uv sync
Using CPython 3.12.11 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Removed virtual environment at: .venv # WHYYYYY!?!?!?!
Creating virtual environment at: .venv
Resolved 14 packages in 3ms
Installed 13 packages in 19ms
 + annotated-types==0.7.0
 + anyio==4.10.0
 + certifi==2025.8.3
 + h11==0.16.0
 + httpcore==1.0.9
 + httpx==0.28.1
 + idna==3.10
 + ollama==0.5.3
 + pydantic==2.11.7
 + pydantic-core==2.33.2
 + sniffio==1.3.1
 + typing-extensions==4.15.0
 + typing-inspection==0.4.1
$ uv
bash: /Users/antony/tonybot/.venv/bin/uv: No such file or directory
```

I can't put into words how *extremely frustrating* this behaviour is. I already have a good workflow for setting up Python and a venv just how I like it!! For extra context, I am still reeling from having to change my usual venv location from `venv` to `.venv` (I _hate_ having to type the extra character). I am pretty sure `uv` used to respect existing venvs so long as the name was the same but now it seems to just clobber whatever is there.

IIRC there's a command line flag (called like ~`--existing` or `--inplace` or something~ it's `--active`) that would make `uv` actually respect the existing venv that it's running in but it's a massive pain to have to remember to include that flag on every single invocation.

Questions:

1. Is this expected behaviour from `uv` (to delete any existing venv?)
2. How should I change my workflow so that `uv` actually helps me?

Related issues:

- https://github.com/astral-sh/uv/issues/6204
- https://github.com/astral-sh/uv/issues/6612
- https://github.com/astral-sh/uv/issues/11273

---

_Comment by @antonysouthworth-halter on 2025-08-31 09:06_

UPDATE: I had a `.python-version` floating around for some reason. When I delete this and re-run `uv` it is now behaving as expected.

```
$ uv --no-managed-python --no-python-downloads sync --active --dry-run --verbose 
DEBUG uv 0.8.14 (af856fb88 2025-08-28)
DEBUG Found project root: `/Users/antony/tonybot`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/antony/tonybot`
DEBUG Reading Python requests from version file at `/Users/antony/tonybot/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version does not satisfy the request: `Python 3.12`
DEBUG Searching for Python 3.12 in search path
DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/Users/antony/tonybot/.venv/bin/python3` (first executable in the search path)
DEBUG Ignoring Python interpreter at `/Users/antony/tonybot/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/Users/antony/tonybot/.venv/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/Users/antony/tonybot/.venv/bin/python`: system interpreter required
DEBUG Failed to inspect Python interpreter from search path at `/Users/antony/.asdf/shims/python3.12` 
DEBUG Skipping bad interpreter at /Users/antony/.asdf/shims/python3.12 from search path: Querying Python at `/Users/antony/.asdf/shims/python3.12` failed with exit status exit status: 126

[stderr]
No version is set for command python3.12
Consider adding one of the following versions in your config file at /Users/antony/tonybot/.tool-versions
python 3.12.7
python 3.12.6

DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/Users/antony/.asdf/shims/python3` (search path)
DEBUG Skipping interpreter at `/Users/antony/.asdf/installs/python/3.13.7/bin/python3` from search path: does not satisfy request `3.12`
DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/Users/antony/.asdf/shims/python` (search path)
DEBUG Skipping interpreter at `/Users/antony/.asdf/installs/python/3.13.7/bin/python` from search path: does not satisfy request `3.12`
DEBUG Found `cpython-3.12.11-macos-aarch64-none` at `/opt/homebrew/bin/python3.12` (search path)
Using CPython 3.12.11 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
DEBUG Using base executable for virtual environment: /opt/homebrew/opt/python@3.12/bin/python3.12
DEBUG Released lock at `/var/folders/mq/s4d2zdnn0m1fgd7drqpz33h80000gp/T/uv-97baab62b3491e7d.lock`
DEBUG Acquired lock for `/Users/antony/.cache/uv/builds-v0/.tmp7AwFjh`
Would replace project environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: tonybot @ file:///Users/antony/tonybot
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 14 packages in 4ms
Found up-to-date lockfile at: uv.lock
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: ollama==0.5.3
DEBUG Registry requirement already cached: httpx==0.28.1
DEBUG Registry requirement already cached: pydantic==2.11.7
DEBUG Registry requirement already cached: anyio==4.10.0
DEBUG Registry requirement already cached: certifi==2025.8.3
DEBUG Registry requirement already cached: httpcore==1.0.9
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: annotated-types==0.7.0
DEBUG Registry requirement already cached: pydantic-core==2.33.2
DEBUG Registry requirement already cached: typing-extensions==4.15.0
DEBUG Registry requirement already cached: typing-inspection==0.4.1
DEBUG Registry requirement already cached: sniffio==1.3.1
DEBUG Registry requirement already cached: h11==0.16.0
Would install 13 packages
 + annotated-types==0.7.0
 + anyio==4.10.0
 + certifi==2025.8.3
 + h11==0.16.0
 + httpcore==1.0.9
 + httpx==0.28.1
 + idna==3.10
 + ollama==0.5.3
 + pydantic==2.11.7
 + pydantic-core==2.33.2
 + sniffio==1.3.1
 + typing-extensions==4.15.0
 + typing-inspection==0.4.1
DEBUG Released lock at `/Users/antony/.cache/uv/builds-v0/.tmp7AwFjh/.lock`
```

```
$ rm .python-version 
remove .python-version? y
.python-version
```

```
$ uv --no-managed-python --no-python-downloads sync --active --dry-run --verbose 
DEBUG uv 0.8.14 (af856fb88 2025-08-28)
DEBUG Found project root: `/Users/antony/tonybot`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/antony/tonybot`
DEBUG No Python version file found in workspace: /Users/antony/tonybot
DEBUG Using Python request `>=3.12` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.12`
DEBUG Released lock at `/var/folders/mq/s4d2zdnn0m1fgd7drqpz33h80000gp/T/uv-97baab62b3491e7d.lock`
DEBUG Acquired lock for `.venv`
Would use project environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: tonybot @ file:///Users/antony/tonybot
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 14 packages in 3ms
Found up-to-date lockfile at: uv.lock
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: ollama==0.5.3
DEBUG Registry requirement already cached: httpx==0.28.1
DEBUG Registry requirement already cached: pydantic==2.11.7
DEBUG Registry requirement already cached: anyio==4.10.0
DEBUG Registry requirement already cached: certifi==2025.8.3
DEBUG Registry requirement already cached: httpcore==1.0.9
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: annotated-types==0.7.0
DEBUG Identified uncached distribution: pydantic-core==2.33.2
DEBUG Registry requirement already cached: typing-extensions==4.15.0
DEBUG Registry requirement already cached: typing-inspection==0.4.1
DEBUG Registry requirement already cached: sniffio==1.3.1
DEBUG Registry requirement already cached: h11==0.16.0
DEBUG Preserving seed package: pip==25.2
DEBUG Preserving seed package: uv==0.8.14
Would download 1 package
Would install 13 packages
 + annotated-types==0.7.0
 + anyio==4.10.0
 + certifi==2025.8.3
 + h11==0.16.0
 + httpcore==1.0.9
 + httpx==0.28.1
 + idna==3.10
 + ollama==0.5.3
 + pydantic==2.11.7
 + pydantic-core==2.33.2
 + sniffio==1.3.1
 + typing-extensions==4.15.0
 + typing-inspection==0.4.1
DEBUG Released lock at `/Users/antony/tonybot/.venv/.lock`
```

```
$ uv sync --active
Resolved 14 packages in 3ms
Prepared 1 package in 140ms
Installed 13 packages in 15ms
 + annotated-types==0.7.0
 + anyio==4.10.0
 + certifi==2025.8.3
 + h11==0.16.0
 + httpcore==1.0.9
 + httpx==0.28.1
 + idna==3.10
 + ollama==0.5.3
 + pydantic==2.11.7
 + pydantic-core==2.33.2
 + sniffio==1.3.1
 + typing-extensions==4.15.0
 + typing-inspection==0.4.1
```

---

_Comment by @antonysouthworth-halter on 2025-08-31 09:08_

I guess if you are using `asdf` and `uv` together, `uv` sees the `.python-version` file and thinks that gives it carte-blanche to rip up any existing venv, even when using `--active`.

Might be a bug? As a user I would expect `--active` to always use the active env.

---

_Comment by @konstin on 2025-09-01 07:16_

In difference to `uv pip install`, the idea of `uv sync` is that it manages your venv. A `uv sync` ensure that the venv contains exactly the packages declared in your `pyproject.toml`, through the locked `uv.lock` versions, i.e. venv management is automatic. With `uv run`, this also removes the need to activate the venv. This makes the venv itself ephemeral, with a warm cache it can be recreated from the lockfile in <1s. `uv sync` also respects `.python-version`, ensuring that the venv matches the pin of the project.

We highly recommend to install uv outside that venv, with uv's curl installer, with asdf or with pipx as a global tool; having uv in the environment it is also managing can make uv fail.

`uv sync --active` recreating the active venv is indeed wrong, we either need to fix the behavior to not recreate or update the documentation to not say `Instead of creating or updating the virtual environment`.

---

_Renamed from "uv clobbered my existing venv" to " `uv sync --active` can recreate active venv" by @konstin on 2025-09-01 07:16_

---

_Label `bug` added by @konstin on 2025-09-01 07:16_

---

_Comment by @charliermarsh on 2025-09-01 13:13_

> `uv sync --active` recreating the active venv is indeed wrong.

So the right behavior here would be: raise an error?

---

_Comment by @zanieb on 2025-09-01 14:02_

> uv sync --active recreating the active venv is indeed wrong, we either need to fix the behavior to not recreate or update the documentation to not say Instead of creating or updating the virtual environment.

I'm not sure I agree? You're changing the target path with `--active` so I think it's fair for uv to manage the environment? It's the same as setting `UV_PROJECT_ENVIRONMENT=$VIRTUAL_ENV`

---

_Comment by @konstin on 2025-09-01 14:02_

I think so, though I want to hear from @zanieb first.

---

_Comment by @zanieb on 2025-09-01 14:05_

It's plausible we should special-case `--active` to never change the Python version of your environment, I just worry about special-cases making it harder to understand how uv manages environments.



---

_Comment by @konstin on 2025-09-01 14:06_

I find it surprising that uv will remove (and recreate) an active environment when using `--active`, this reduces the flag to selecting a different location for a venv; If we do want to go with that we should clarify the docs for `--active` that it only changes the location of the environment, while it is still managed by uv and uv may recreate it.

---

_Comment by @zanieb on 2025-09-01 14:07_

Why is that surprising given we could also remove all the packages in the environment?

---

_Comment by @konstin on 2025-09-01 14:20_

From an option called `--active` and a description "Sync dependencies to the active virtual environment.", my expectation would be that it uses that environment and only gives me package versions matching , especially when running with `--inexact`. This may be a naming problem: With `UV_PROJECT_ENVIRONMENT` being given a path, I do expect it to use that location and am not surprised if it (re)creates an environment at the given path (despite them doing the same thing).

---

_Comment by @alechouse97 on 2025-09-16 13:08_

I am seeing similar behavior when trying to `uv sync --active` into an activated conda environment - uv ignores this environment, creates a venv, and syncs the package there. Is this expected behavior?

---

_Comment by @zanieb on 2025-09-16 13:18_

@alechouse97 yes it's expected (though we may change it), if you turn on verbose logs you'll see the reason the environment is considered incompatible. Usually it's because of a different Python version request.

---

_Comment by @soufianeamini on 2025-09-25 19:21_

I've encountered a similar issue, which is that I had an active venv two folders up, and then running `uv sync --active` in the current directory that contains a different pyproject.toml as well ends up deleting the .venv that exists two folders up and creating a new one in the current directory.

I think raising an error like Charlie mentioned might be a good solution. Not sure if this scenario I mentioned is intended to happen.

---

_Comment by @antonysouthworth-halter on 2025-10-05 20:34_

> UPDATE: I had a `.python-version` floating around for some reason. When I delete this and re-run `uv` it is now behaving as expected.

...And I think I just found out how; I believe the `.python-version` is added by `uv init`.

So what was happening to me is:

1. Use `asdf set python ...` to set Python version.
2. Use `python -m venv venv` to create venv; then I activate it.
3. Use `uv init` -> creates `.python-version`.
4. Use `uv add --active ...` -> `uv` ignores `--active` flag.

Just another minor note, but the other thing I hate about the `--active` flag is it's not obvious which commands do/don't need it. Again, I'd prefer if `uv` just used the active venv by default; though at the same time I admit 1) I'm very much "stuck in my ways" and 2) given _most_ people probably don't have strong opinions on their venv management (they just want things to Just Work™️) clobbering any existing crap and making the env fully "uv-managed" probably saves more people in the aggregate.  

---

_Comment by @antonysouthworth-halter on 2025-10-12 20:47_

Hit this again. This time I suspect the reason is because of the mismatch between the Python version that the venv was created with and the Python version specified in `pyproject.toml`

```shell
$ python -m venv venv
$ . venv/bin/activate
(venv) $ python -m pip install --upgrade pip uv
# ... omitted for clarity
(venv) $ cat pyproject.toml | grep requires-python
requires-python = ">=3.12"
(venv) $ python --version
Python 3.10.12
(venv) $ uv sync --active
Using CPython 3.12.10 interpreter at: /usr/bin/python3.12
Removed virtual environment at: venv
Creating virtual environment at: venv
Resolved 139 packages in 1ms
# ... truncated
```

I think this case should quite clearly be an error where `uv` should refuse to continue.

Desired behaviour (imagine the shell exprs had been expanded)

```
(venv) $ uv sync --active
You used the --active flag which requires us to use the active virtual environment at $VIRTUAL_ENV -- but this virtual environment is using:

    $($VIRTUAL_ENV/bin/python --version)

which is incompatible with the project's requirements in `pyproject.toml`:

    $(grep 'requires-python' pyproject.toml)

We recommend letting uv manage your Python version and virtual environment, read more at $URL.

Alternatively (if you prefer managing the environment yourself) you should re-create the virtual environment using a compatible version of Python.
```

EDIT: just to confirm: yes, re-creating the venv with a compatible Python version works properly. So lends some credence to the mismatch theory.

---

_Comment by @zanieb on 2025-10-12 21:15_

@antonysouthworth-halter With the `-v` flag we're usually pretty clear about what is causing the environment to be considered invalid.

---

_Label `breaking` added by @zanieb on 2025-10-12 21:15_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:17_

---

_Comment by @zanieb on 2025-10-12 21:22_

We can consider this changing this for v0.10, though I'm wary that changing this behavior will break some use-cases and make uv's management of project environments less consistent and harder to reason about.

We'll also need to decide if we should error when the Python versions differ or just use the Python version found in the environment.

---

_Comment by @antonysouthworth-halter on 2025-10-13 00:28_

On "reasoning about uv's environment management": having your venv blown away **without warning** is _very_ hard to reason about (and extremely frustrating; thankfully `uv`s cache means re-creating the venv is quite fast <3). Not sure if that reads snarky or not; apologies if so, not my intention. I agree having reasonable (or "reason-about-able") behaviour is extremely important so sharing my perspective on that. (EDIT: maybe interpret this paragraph as "`uv`s project environment management is _already_ hard to reason about, or at least it is for me").

On "what to do about it": agree. My mind immediately went to erroring out as above on the grounds that the user has effectively given `uv` conflicting instructions. On the other hand, one could argue that, if `--active` means "use existing packages in the environment", then it's a somewhat natural extension to include "use existing python from the environment too".

A backwards-compatible fix might be to simply emit a warning "hey, you used `--active` but the venv is no good because $reason, we will re-create it" and proceed with the current behaviour. I'd probably even be happy enough if `uv` just prompted me for confirmation before proceeding with the current behaviour (though that might be considered a breaking change for you guys since the program would be waiting for input at a new location).

Personally I would prefer the erroring approach since if I get that warning I am just going to cancel the operation anyway.

---

_Referenced in [astral-sh/uv#16631](../../astral-sh/uv/issues/16631.md) on 2025-11-07 14:42_

---

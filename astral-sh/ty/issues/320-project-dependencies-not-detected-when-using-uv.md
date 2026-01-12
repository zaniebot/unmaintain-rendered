```yaml
number: 320
title: "Project dependencies not detected when using `uv run --with ty`"
type: issue
state: closed
author: zanieb
labels:
  - imports
  - uv
assignees: []
created_at: 2025-05-11T17:00:32Z
updated_at: 2025-05-28T15:27:03Z
url: https://github.com/astral-sh/ty/issues/320
synced_at: 2026-01-12T15:54:22Z
```

# Project dependencies not detected when using `uv run --with ty`

---

_@zanieb_

> Steps to reproduce (CPython 3.13.2, uv 0.7.3, ty 0.0.0-alpha.8):
> 
> ```
> uv init tmp
> cd tmp
> uv add httpx
> echo 'import httpx' >> main.py
> uv run --with ty ty check
> ```
> ty says:
> ```
> error[unresolved-import]: Cannot resolve imported module `httpx`
> ```
> it works fine if you `uv add ty`, but doesn't work when you `uv run --with ty ty check`. `uv run --with mypy mypy .` works fine and finds the import.
> 
> `uv run --with ty ty check --verbose --verbose`
> 
> ```
> 2025-05-11 12:51:51.281544701 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
> 2025-05-11 12:51:51.281577281 DEBUG Version: 0.0.0-alpha.8
> 2025-05-11 12:51:51.281602471 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
> 2025-05-11 12:51:51.281703641 DEBUG Searching for a project in '/home/collin/tmp'
> 2025-05-11 12:51:51.281806121 DEBUG Resolving requires-python constraint: `>=3.13`
> 2025-05-11 12:51:51.281832661 DEBUG Resolved requires-python constraint to: 3.13
> 2025-05-11 12:51:51.281854762 DEBUG Project without `tool.ty` section: '/home/collin/tmp'
> 2025-05-11 12:51:51.281868452 DEBUG Searching for a user-level configuration at `/home/collin/.config/ty/ty.toml`
> 2025-05-11 12:51:51.281893422 INFO Defaulting to python-platform `linux`
> 2025-05-11 12:51:51.281917742 INFO Python version: Python 3.13, platform: linux
> 2025-05-11 12:51:51.281930492 DEBUG Adding first-party search path '/home/collin/tmp'
> 2025-05-11 12:51:51.281947742 DEBUG Using vendored stdlib
> 2025-05-11 12:51:51.282668282 DEBUG Discovering site-packages paths from sys-prefix `/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY` (`VIRTUAL_ENV` environment variable')
> 2025-05-11 12:51:51.282701342 DEBUG Attempting to parse virtual environment metadata at '/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY/pyvenv.cfg'
> 2025-05-11 12:51:51.282745112 DEBUG Searching for site-packages directory in `sys.prefix` path `/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY`
> 2025-05-11 12:51:51.282753712 DEBUG Resolved site-packages directories for this virtual environment are: ["/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY/lib/python3.13/site-packages"]
> 2025-05-11 12:51:51.282758592 DEBUG Adding site-packages search path '/home/collin/.cache/uv/archive-v0/aA7ze3NeJgM8fooLQjSvY/lib/python3.13/site-packages'
> 2025-05-11 12:51:51.282942792 DEBUG Starting main loop
> Checking ------------------------------------------------------------ 0/0 files                                                                                                    2025-05-11 12:51:51.283013262 DEBUG Waiting for next main loop message.
> 2025-05-11 12:51:51.283144902 DEBUG Checking project 'tmp'
> 2025-05-11 12:51:51.285666824 INFO Indexed 1 file(s)
> Checking ------------------------------------------------------------ 0/1 files                                                                                                    2025-05-11 12:51:51.286172246 DEBUG Checking file '/home/collin/tmp/main.py'
> 2025-05-11 12:51:51.316921754 DEBUG Resolving dynamic module resolution paths
> 2025-05-11 12:51:51.317122364 DEBUG Module `httpx` not found in search paths
> error[unresolved-import]: Cannot resolve imported module `httpx`
> ``` 

 _Originally posted by @collinanderson in [#171](https://github.com/astral-sh/ty/issues/171#issuecomment-2870001921)_

---

_Comment by @zanieb on 2025-05-11 17:02_

I think the logs are pretty clear that the project virtual environment's site packages aren't included (the environment in the cache directory is the `--with` layer). Why did you think this would work @AlexWaygood ?

---

_Label `imports` added by @MichaReiser on 2025-05-12 06:32_

---

_Comment by @MichaReiser on 2025-05-12 06:33_

@zanieb does that mean uv uses two venvs? Does uv link one venv into the other (e.g. by using a `pth` file or similar? 

Are there other tools that support such layered venvs?

---

_Label `imports` removed by @MichaReiser on 2025-05-12 06:33_

---

_Label `imports` added by @MichaReiser on 2025-05-12 06:33_

---

_Label `uv` added by @MichaReiser on 2025-05-12 06:34_

---

_Comment by @zanieb on 2025-05-12 14:34_

Yes, they're layered as linked here https://github.com/astral-sh/ty/issues/171#issuecomment-2869953949 using a `pth` file.

Support varies, it depends on how they implement site discovery presumably?

---

_Comment by @carljm on 2025-05-12 14:40_

We support `pth` files, but not `pth` files that run arbitrary Python code (in this case `import site; site.addsitedir(...)`, which is what `uv` does to layer venvs.) It's plausible that we could support that specific pattern in order to make this case work.

---

_Comment by @MichaReiser on 2025-05-12 14:46_

We should probably add a debug message when ty skips `.pth` files here

https://github.com/astral-sh/ruff/blob/316e406ca46704b308466a82630a10ddff8c9c35/crates/ty_python_semantic/src/module_resolver/resolver.rs#L501-L516

---

_Comment by @collinanderson on 2025-05-12 15:08_

`uv run --with 'pyright[nodejs]' pyright` also works fine. Peaking at `uv run --with 'pyright[nodejs]' pyright --verbose`, it looks like they ultimately run the python interpreter to determine the correct paths.

---

_Comment by @AlexWaygood on 2025-05-12 20:33_

> Why did you think this would work [@AlexWaygood](https://github.com/AlexWaygood) ?

I didn't look at the code you linked yesterday as I was on my phone, and I had the wrong intuition about what the term "nested venv" would mean! I assumed that all the venvs from the parent venv would be installed directly into the nested venv's `site-packages` directory, but it appears that that's not the case.

---

_Comment by @AlexWaygood on 2025-05-12 20:35_

> We support `pth` files, but not `pth` files that run arbitrary Python code (in this case `import site; site.addsitedir(...)`, which is what `uv` does to layer venvs.) It's plausible that we could support that specific pattern in order to make this case work.

@zanieb I wonder if uv could make this a little easier for ty (and other static tools that want to support these nested venvs) by adding a key to the pyvenv.cfg file for the nested venv? Something like

```cfg
extends = "path/to/parent/venv"
```

We already implement `pyvenv.cfg` parsing in ty -- if we saw that key in the pyvenv.cfg file, it would be pretty trivial for us to add the `site-packages` from the parent venv as a search path as well as the one in the nested venv.

---

_Comment by @zanieb on 2025-05-13 20:28_

It's definitely plausible. Is it bad form to go beyond the specification there? Should we try to standardize something? How do you compare this to just adding support for reading that particular Python idiom adding to `site`?

---

_Comment by @AlexWaygood on 2025-05-13 20:36_

> Is it bad form to go beyond the specification there?

From what I could tell when adding the original `site-packages` support to ty, there is already wide variation between different virtual-environment creation tools in terms of what additional keys they provide in the pyvenv.cfg file. Here's three different pyvenv.cfg files, for comparison, all created using the same Python executable. Some keys are the same across all three, but many are not:

## 1. Created by the stdlib venv module:

```cfg
home = /Users/alexw/.pyenv/versions/3.12.4/bin
include-system-site-packages = false
version = 3.12.4
executable = /Users/alexw/.pyenv/versions/3.12.4/bin/python3.12
command = /Users/alexw/.pyenv/versions/3.12.4/bin/python3 -m venv /Users/alexw/dev/ruff/venv2
```

## 2. Created by the third-party `virtualenv` package:

```cfg
home = /Users/alexw/.pyenv/versions/3.12.4/bin
implementation = CPython
version_info = 3.12.4.final.0
virtualenv = 20.26.3
include-system-site-packages = false
base-prefix = /Users/alexw/.pyenv/versions/3.12.4
base-exec-prefix = /Users/alexw/.pyenv/versions/3.12.4
base-executable = /Users/alexw/.pyenv/versions/3.12.4/bin/python3.12
```

## 3. Created by uv:

```cfgf
home = /Users/alexw/.pyenv/versions/3.12.4/bin
implementation = CPython
uv = 0.2.32
version_info = 3.12.4
include-system-site-packages = true
relocatable = false
prompt = ruff
```

Given this precedent, I think it would be absolutely fine for us to add new experimental keys to uv, even if they're not yet standardised. Obviously better standards here would also be fantastic, though!

> How do you compare this to just adding support for reading that particular Python idiom adding to `site`?

I'd much prefer a static key (that we could attempt to standardise in the future) in a known location that we already parse (the pyvenv.cfg file) rather than checking each pth file in `site-packages` and attempting to parse Python source code in there to see if it matches something resembling this idiom.

---

_Comment by @AlexWaygood on 2025-05-22 18:26_

> [@zanieb](https://github.com/zanieb) I wonder if uv could make this a little easier for ty (and other static tools that want to support these nested venvs) by adding a key to the pyvenv.cfg file for the nested venv?

I filed https://github.com/astral-sh/uv/pull/13598 to implement this feature over at uv

---

_Comment by @karlicoss on 2025-05-26 00:15_

Just my two cents -- I initially tried using `uv run --with ty ty check src/testty` by analogy with `uv run --with mypy mypy src/testty` -- the latter works even if the project doesn't have mypy in pyproject in the first place. So was pretty confused that ty couldn't detect dependencies!

Globally installed ty via `uvx ty` works, I just didn't expect that to work initially since mypy usually needs to be in the same venv as whatever you're running agains (unless you're messing with `MYPYPATH`). I guess the downside is that it won't sync project automatically (which maybe isn't a huge deal unless you change dependencies really often).

`uv add --dev ty` as suggested in https://github.com/astral-sh/ty/blob/main/docs/README.md#installation works as expected though!

---

_Closed by @AlexWaygood on 2025-05-28 14:55_

---

_Comment by @AlexWaygood on 2025-05-28 14:59_

We've now implemented a fix for this, but it currently requires a uv feature that only exists on uv's `main` branch _and_ a ty feature that only exists on ty's `main` branch. Once uv and ty have both cut new releases, you'll need to upgrade them both for everything here to work as expected.

The two relevant commits are:
- https://github.com/astral-sh/uv/commit/7bba3d00d4ad1fb3daba86b98eb25d8d9e9836ae
- https://github.com/astral-sh/ruff/commit/a5ebb3f3a2b347849d0c195884ea944fb690b187

---

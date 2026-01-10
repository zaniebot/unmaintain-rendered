```yaml
number: 1043
title: ty does not find nacl.public with poetry and python 3.13
type: issue
state: closed
author: erdnaxeli
labels:
  - imports
assignees: []
created_at: 2025-08-18T12:48:08Z
updated_at: 2025-08-19T13:46:33Z
url: https://github.com/astral-sh/ty/issues/1043
synced_at: 2026-01-10T02:06:24Z
```

# ty does not find nacl.public with poetry and python 3.13

---

_Issue opened by @erdnaxeli on 2025-08-18 12:48_

### Summary

Steps to reproduces (needs python 3.13):
```
poetry init
poetry add pynacl ty
echo "import nacl.public" > t.py
poetry run ty check
```

Expected: no error.

Result:
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                         
error[unresolved-import]: Cannot resolve imported module `nacl.public`
 --> t.py:1:8
  |
1 | import nacl.public
  |        ^^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```

The error does not happen with Python 3.12, nor if using uv to manage the project (either with Python 3.12 or 3.13).

### Version

ty 0.0.1-alpha.18

Poetry (version 2.1.4)

---

_Label `imports` added by @AlexWaygood on 2025-08-18 12:51_

---

_Comment by @erdnaxeli on 2025-08-18 12:53_

It does not work neither if I try to activate the venv first or if I specify the venv's python with `--python`.

---

_Comment by @AlexWaygood on 2025-08-18 13:00_

Hmm, I cannot reproduce this locally. Here's what I get if I try out the commands you specified. The only differences to what you did are:
- I don't have poetry installed globally, so I'm invoking it via `uvx` -- but I don't think that should make a difference
- I ran `poetry run ty check -vv` to print debug information, rather than just `poetry run ty check`

```sh
~/dev % mkdir poetry-repro     
~/dev % cd poetry-repro                                     
~/dev/poetry-repro % uvx poetry init                                                         
Installed 43 packages in 40ms

This command will guide you through creating your pyproject.toml config.

Package name [poetry-repro]:  
Version [0.1.0]:  
Description []:  
Author [Alex Waygood <alex.waygood@gmail.com>, n to skip]:  
License []:  
Compatible Python versions [>=3.13]:  

Would you like to define your main dependencies interactively? (yes/no) [yes] n
Would you like to define your development dependencies interactively? (yes/no) [yes] n
Generated file

[project]
name = "poetry-repro"
version = "0.1.0"
description = ""
authors = [
    {name = "Alex Waygood",email = "alex.waygood@gmail.com"}
]
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
]


[build-system]
requires = ["poetry-core>=2.0.0,<3.0.0"]
build-backend = "poetry.core.masonry.api"


Do you confirm generation? (yes/no) [yes] y
~/dev/poetry-repro % uvx poetry add pynacl ty
Creating virtualenv poetry-repro-nj2iZ4TX-py3.13 in /Users/alexw/Library/Caches/pypoetry/virtualenvs
Using version ^1.5.0 for pynacl
Using version ^0.0.1a18 for ty

Updating dependencies
Resolving dependencies... (0.5s)

Package operations: 4 installs, 0 updates, 0 removals

  - Installing pycparser (2.22)
  - Installing cffi (1.17.1)
  - Installing ty (0.0.1a18)
  - Installing pynacl (1.5.0)

Writing lock file
~/dev/poetry-repro % echo "import nacl.public" > t.py
~/dev/poetry-repro % uvx poetry run ty check -vv
2025-08-18 13:56:27.738617 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-08-18 13:56:27.743808 DEBUG Version: 0.0.1-alpha.18 (d697cc092 2025-08-14)
2025-08-18 13:56:27.743846 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-08-18 13:56:27.743859 DEBUG Searching for a project in '/Users/alexw/dev/poetry-repro'
2025-08-18 13:56:27.743908 DEBUG Resolving requires-python constraint: `>=3.13`
2025-08-18 13:56:27.743914 DEBUG Resolved requires-python constraint to: 3.13
2025-08-18 13:56:27.743939 DEBUG Project without `tool.ty` section: '/Users/alexw/dev/poetry-repro'
2025-08-18 13:56:27.743945 DEBUG Searching for a user-level configuration at `/Users/alexw/.config/ty/ty.toml`
2025-08-18 13:56:27.744194 INFO Defaulting to python-platform `darwin`
2025-08-18 13:56:27.744199 DEBUG Resolving `VIRTUAL_ENV` environment variable: /Users/alexw/Library/Caches/pypoetry/virtualenvs/poetry-repro-nj2iZ4TX-py3.13
2025-08-18 13:56:27.744234 DEBUG Attempting to parse virtual environment metadata at '/Users/alexw/Library/Caches/pypoetry/virtualenvs/poetry-repro-nj2iZ4TX-py3.13/pyvenv.cfg'
2025-08-18 13:56:27.744263 DEBUG Searching for site-packages directory in sys.prefix /Users/alexw/Library/Caches/pypoetry/virtualenvs/poetry-repro-nj2iZ4TX-py3.13
2025-08-18 13:56:27.744273 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/alexw/Library/Caches/pypoetry/virtualenvs/poetry-repro-nj2iZ4TX-py3.13/lib/python3.13/site-packages"})
2025-08-18 13:56:27.744362 DEBUG Searching for real stdlib directory in sys.prefix /Users/alexw/Library/Application Support/uv/python/cpython-3.13.6-macos-aarch64-none
2025-08-18 13:56:27.744377 DEBUG Resolved real stdlib path for this virtual environment is: /Users/alexw/Library/Application Support/uv/python/cpython-3.13.6-macos-aarch64-none/lib/python3.13
2025-08-18 13:56:27.744383 DEBUG Including `.` in `environment.root`
2025-08-18 13:56:27.744389 DEBUG Adding first-party search path `/Users/alexw/dev/poetry-repro`
2025-08-18 13:56:27.744393 DEBUG Using vendored stdlib
2025-08-18 13:56:27.744509 DEBUG Adding site-packages search path `/Users/alexw/Library/Caches/pypoetry/virtualenvs/poetry-repro-nj2iZ4TX-py3.13/lib/python3.13/site-packages`
2025-08-18 13:56:27.744515 INFO Python version: Python 3.13, platform: darwin
2025-08-18 13:56:27.744694 DEBUG Starting main loop
2025-08-18 13:56:27.744701 DEBUG Waiting for next main loop message.
2025-08-18 13:56:27.744753 DEBUG Checking all files in project 'poetry-repro'
2025-08-18 13:56:27.748854 INFO Indexed 1 file(s) in 0.004s
2025-08-18 13:56:27.748939 DEBUG Checking file '/Users/alexw/dev/poetry-repro/t.py'
2025-08-18 13:56:27.74902 DEBUG Resolving dynamic module resolution paths
2025-08-18 13:56:27.749163 DEBUG Checking all files took 0.000s
All checks passed!
2025-08-18 13:56:27.749176 DEBUG Exiting main loop
```

Would you be able to repeat things at your end and run `ty check` with `-vv`, then paste the output here? We can see from the debug logs on my end that ty has successfully resolved the virtual environment due to the `poetry run` command setting the `VIRTUAL_ENV` variable. Maybe that's not working correctly on your end for some reason?

---

_Label `needs-mre` added by @AlexWaygood on 2025-08-18 13:00_

---

_Comment by @erdnaxeli on 2025-08-18 13:30_

I cannot reproduce it either using poetry through `uvx --python python3.13` 🤔 

Here is the debug output while using poetry directly:
```
2025-08-18 15:24:05.120290431 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-08-18 15:24:05.120876049 DEBUG Version: 0.0.1-alpha.18
2025-08-18 15:24:05.120895325 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-08-18 15:24:05.120954127 DEBUG Searching for a project in '/home/erdnaxeli/bacasable/2025/08/tybug'
2025-08-18 15:24:05.121036742 DEBUG Resolving requires-python constraint: `>=3.13`
2025-08-18 15:24:05.12104413 DEBUG Resolved requires-python constraint to: 3.13
2025-08-18 15:24:05.121079356 DEBUG Project without `tool.ty` section: '/home/erdnaxeli/bacasable/2025/08/tybug'
2025-08-18 15:24:05.121088751 DEBUG Searching for a user-level configuration at `/home/erdnaxeli/.config/ty/ty.toml`
2025-08-18 15:24:05.121660963 INFO Defaulting to python-platform `linux`
2025-08-18 15:24:05.121679281 DEBUG Resolving `VIRTUAL_ENV` environment variable: /home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13
2025-08-18 15:24:05.12169558 DEBUG Attempting to parse virtual environment metadata at '/home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13/pyvenv.cfg'
2025-08-18 15:24:05.121720051 DEBUG Searching for site-packages directory in sys.prefix /home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13
2025-08-18 15:24:05.121738119 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13/lib/python3.13/site-packages"})
2025-08-18 15:24:05.122469023 DEBUG Searching for real stdlib directory in sys.prefix /usr
2025-08-18 15:24:05.12247831 DEBUG Resolved real stdlib path for this virtual environment is: /usr/lib/python3.13
2025-08-18 15:24:05.122493303 DEBUG Including `.` in `environment.root`
2025-08-18 15:24:05.122503866 DEBUG Adding first-party search path `/home/erdnaxeli/bacasable/2025/08/tybug`
2025-08-18 15:24:05.122516236 DEBUG Using vendored stdlib
2025-08-18 15:24:05.122723848 DEBUG Adding site-packages search path `/home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13/lib/python3.13/site-packages`
2025-08-18 15:24:05.12273514 INFO Python version: Python 3.13, platform: linux
2025-08-18 15:24:05.123024607 DEBUG Starting main loop
2025-08-18 15:24:05.123038151 DEBUG Waiting for next main loop message.
2025-08-18 15:24:05.12309942 DEBUG Checking all files in project 'tybug'
2025-08-18 15:24:05.124822463 INFO Indexed 2 file(s) in 0.002s
2025-08-18 15:24:05.125018629 DEBUG Checking file '/home/erdnaxeli/bacasable/2025/08/tybug/tybug/__init__.py'
2025-08-18 15:24:05.125028493 DEBUG Checking file '/home/erdnaxeli/bacasable/2025/08/tybug/t.py'
2025-08-18 15:24:05.125155835 DEBUG Resolving dynamic module resolution paths
2025-08-18 15:24:05.12523056 DEBUG Module `nacl.public` not found in search paths
2025-08-18 15:24:05.12526192 DEBUG Checking all files took 0.000s
error[unresolved-import]: Cannot resolve imported module `nacl.public`
 --> t.py:1:8
  |
1 | import nacl.public
  |        ^^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-08-18 15:24:05.125331657 DEBUG Exiting main loop
```

We can see that `ty` resolves the site-packages dir to "/home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13/**lib**/python3.13/site-packages", which exists but does not contains `nacl`. The actual `nacl` files are here: "/home/erdnaxeli/.cache/pypoetry/virtualenvs/tybug-LJZ3gUnO-py3.13/**lib64**/python3.13/site-packages/nacl/__init__.py". The issue is `lib` vs `lib64`.

---

_Comment by @erdnaxeli on 2025-08-18 13:31_

In the `uvx poetry` version, the package `nacl` is installed in the `lib/` version of site-packages, and the `lib64/` folder does not exist.

---

_Comment by @erdnaxeli on 2025-08-18 13:33_

For the completeness, I installed poetry using pipx.

```
$ pipx list
venvs are in /home/erdnaxeli/.local/share/pipx/venvs
apps are exposed on your $PATH at /home/erdnaxeli/.local/bin
manual pages are exposed at /home/erdnaxeli/.local/share/man
   package poetry 2.1.4, installed using Python 3.13.1
    - poetry
```

---

_Comment by @AlexWaygood on 2025-08-18 13:52_

hmmmmm... this comment feels quite relevant here 😅 https://github.com/astral-sh/ruff/blob/e4f1b587cc2a5764e3fd6bda4e48ffedd111f30d/crates/ty_python_semantic/src/site_packages.rs#L994-L1012

I'll take a look and try to see if I can figure out what's incorrect about my reasoning there...

---

_Comment by @carljm on 2025-08-18 15:37_

It may be that the restriction that non-namespace packages can only exist in one place means that sometimes an entire package has to go in `lib64` if it contains any C extensions? Not really familiar with how this is all handled in installers, and clearly it's not necessarily consistent, even for a given package.

---

_Label `needs-mre` removed by @carljm on 2025-08-18 15:37_

---

_Comment by @AlexWaygood on 2025-08-18 17:40_

@erdnaxeli could you also share the `-vv` output you get if you invoke poetry using `uvx`, like I was doing? I'm curious if the two invocations are using the same Python interpreter under the hood, or if the `uvx` invocation is using a uv-managed installation of Python

---

_Comment by @erdnaxeli on 2025-08-19 07:42_

Here is the output with `uvx --python python3.13 poetry …` (with `--python python 3.13` it uses a python 3.12, I don't know where this version comes from):
```
2025-08-19 09:34:41.406605921 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-08-19 09:34:41.407137406 DEBUG Version: 0.0.1-alpha.18
2025-08-19 09:34:41.407149083 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-08-19 09:34:41.407201981 DEBUG Searching for a project in '/home/erdnaxeli/bacasable/2025/08/ttt'
2025-08-19 09:34:41.407263009 DEBUG Resolving requires-python constraint: `>=3.13`
2025-08-19 09:34:41.407270268 DEBUG Resolved requires-python constraint to: 3.13
2025-08-19 09:34:41.407289518 DEBUG Project without `tool.ty` section: '/home/erdnaxeli/bacasable/2025/08/ttt'
2025-08-19 09:34:41.407297013 DEBUG Searching for a user-level configuration at `/home/erdnaxeli/.config/ty/ty.toml`
2025-08-19 09:34:41.40777077 INFO Defaulting to python-platform `linux`
2025-08-19 09:34:41.407782773 DEBUG Resolving `VIRTUAL_ENV` environment variable: /home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13
2025-08-19 09:34:41.407796493 DEBUG Attempting to parse virtual environment metadata at '/home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13/pyvenv.cfg'
2025-08-19 09:34:41.407820113 DEBUG Searching for site-packages directory in sys.prefix /home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13
2025-08-19 09:34:41.407831707 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13/lib/python3.13/site-packages"})
2025-08-19 09:34:41.407858171 DEBUG Searching for real stdlib directory in sys.prefix /home/linuxbrew/.linuxbrew/Cellar/python@3.13/3.13.7
2025-08-19 09:34:41.407863693 DEBUG Resolved real stdlib path for this virtual environment is: /home/linuxbrew/.linuxbrew/Cellar/python@3.13/3.13.7/lib/python3.13
2025-08-19 09:34:41.407870184 DEBUG Including `.` in `environment.root`
2025-08-19 09:34:41.407874583 DEBUG Adding first-party search path `/home/erdnaxeli/bacasable/2025/08/ttt`
2025-08-19 09:34:41.407878819 DEBUG Using vendored stdlib
2025-08-19 09:34:41.408068946 DEBUG Adding site-packages search path `/home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13/lib/python3.13/site-packages`
2025-08-19 09:34:41.408077469 INFO Python version: Python 3.13, platform: linux
2025-08-19 09:34:41.408373061 DEBUG Starting main loop
2025-08-19 09:34:41.408391293 DEBUG Waiting for next main loop message.
2025-08-19 09:34:41.408458084 DEBUG Checking all files in project 'ttt'
2025-08-19 09:34:41.411240691 INFO Indexed 1 file(s) in 0.003s
2025-08-19 09:34:41.411339779 DEBUG Checking file '/home/erdnaxeli/bacasable/2025/08/ttt/t.py'
2025-08-19 09:34:41.411458953 DEBUG Resolving dynamic module resolution paths
2025-08-19 09:34:41.411620485 DEBUG Checking all files took 0.000s
All checks passed!
2025-08-19 09:34:41.411651711 DEBUG Exiting main loop
```

It seems to use python 3.13 from brew?! Indeed I installed uv through brew, and it seems I also have python3.13 installed with brew.

Interestingly, if I uninstall the python3.13 installed with brew, I get the error again:

```
2025-08-19 09:41:21.419374968 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-08-19 09:41:21.419911072 DEBUG Version: 0.0.1-alpha.18
2025-08-19 09:41:21.419922351 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-08-19 09:41:21.419988625 DEBUG Searching for a project in '/home/erdnaxeli/bacasable/2025/08/ttt'
2025-08-19 09:41:21.420057477 DEBUG Resolving requires-python constraint: `>=3.13`
2025-08-19 09:41:21.42006411 DEBUG Resolved requires-python constraint to: 3.13
2025-08-19 09:41:21.420082005 DEBUG Project without `tool.ty` section: '/home/erdnaxeli/bacasable/2025/08/ttt'
2025-08-19 09:41:21.420090425 DEBUG Searching for a user-level configuration at `/home/erdnaxeli/.config/ty/ty.toml`
2025-08-19 09:41:21.420585521 INFO Defaulting to python-platform `linux`
2025-08-19 09:41:21.420612785 DEBUG Resolving `VIRTUAL_ENV` environment variable: /home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13
2025-08-19 09:41:21.420627658 DEBUG Attempting to parse virtual environment metadata at '/home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13/pyvenv.cfg'
2025-08-19 09:41:21.420650271 DEBUG Searching for site-packages directory in sys.prefix /home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13
2025-08-19 09:41:21.420662772 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13/lib/python3.13/site-packages"})
2025-08-19 09:41:21.421393039 DEBUG Searching for real stdlib directory in sys.prefix /usr
2025-08-19 09:41:21.421400589 DEBUG Resolved real stdlib path for this virtual environment is: /usr/lib/python3.13
2025-08-19 09:41:21.421414522 DEBUG Including `.` in `environment.root`
2025-08-19 09:41:21.421421062 DEBUG Adding first-party search path `/home/erdnaxeli/bacasable/2025/08/ttt`
2025-08-19 09:41:21.421435887 DEBUG Using vendored stdlib
2025-08-19 09:41:21.421671766 DEBUG Adding site-packages search path `/home/erdnaxeli/.cache/pypoetry/virtualenvs/ttt-Gh_YvyGT-py3.13/lib/python3.13/site-packages`
2025-08-19 09:41:21.421683984 INFO Python version: Python 3.13, platform: linux
2025-08-19 09:41:21.421964567 DEBUG Starting main loop
2025-08-19 09:41:21.421975675 DEBUG Waiting for next main loop message.
2025-08-19 09:41:21.422030325 DEBUG Checking all files in project 'ttt'
2025-08-19 09:41:21.424734373 INFO Indexed 1 file(s) in 0.003s
2025-08-19 09:41:21.424825524 DEBUG Checking file '/home/erdnaxeli/bacasable/2025/08/ttt/t.py'
2025-08-19 09:41:21.424942019 DEBUG Resolving dynamic module resolution paths
2025-08-19 09:41:21.424999936 DEBUG Module `nacl.public` not found in search paths
2025-08-19 09:41:21.425043988 DEBUG Checking all files took 0.000s
error[unresolved-import]: Cannot resolve imported module `nacl.public`
 --> t.py:1:8
  |
1 | import nacl.public
  |        ^^^^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-08-19 09:41:21.425139684 DEBUG Exiting main loop
```

---

_Closed by @AlexWaygood on 2025-08-19 11:53_

---

_Comment by @AlexWaygood on 2025-08-19 13:46_

We _believe_ that this was due to poetry doing something that it shouldn't. But, regardless, it's not too hard for us to workaround the bug -- and the logic changes that we have to make to workaround the bug are things we were planning to do anyway in the long term to support othe features.

It should now be fixed in v0.0.1a19 -- please let me know if you're still experiencing the error after upgrading!

---

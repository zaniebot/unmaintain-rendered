---
number: 7506
title: "--no-binary affects later pip install without that flag"
type: issue
state: closed
author: RazerM
labels:
  - question
assignees: []
created_at: 2024-09-18T15:51:24Z
updated_at: 2024-10-21T22:12:12Z
url: https://github.com/astral-sh/uv/issues/7506
synced_at: 2026-01-07T13:12:17-06:00
---

# --no-binary affects later pip install without that flag

---

_Issue opened by @RazerM on 2024-09-18 15:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
**Version**: uv 0.4.11

When `--no-binary` is used, some packages run a compiler during package installation, but others just ship a Python fallback in their sdist.

An example of which is black. If I install it with `--no-binary black`, then a later install without this flag gets the cached copy without compiled extensions – I found this quite surprising:

```
~/uvtmp
❯ uv pip install --no-binary black black
Resolved 6 packages in 690ms
   Built black==24.8.0
Prepared 1 package in 1.54s
Installed 1 package in 12ms
 + black==24.8.0
 
~/uvtmp took 2s
❯ ls .venv/lib/python3.12/site-packages/black/*.so
zsh: no matches found: .venv/lib/python3.12/site-packages/black/*.so

~/uvtmp
❯ uv pip uninstall black
Uninstalled 1 package in 11ms
 - black==24.8.0
 
~/uvtmp
❯ uv pip install black                            # Note lack of --no-binary
Resolved 6 packages in 277ms
Installed 1 package in 11ms
 + black==24.8.0
 
~/uvtmp
❯ ls .venv/lib/python3.12/site-packages/black/*.so
zsh: no matches found: .venv/lib/python3.12/site-packages/black/*.so

~/uvtmp
❯ uv pip uninstall black
Uninstalled 1 package in 11ms
 - black==24.8.0
 
~/uvtmp
❯ uv pip install --refresh-package black black      # refresh is required
Resolved 6 packages in 460ms
Prepared 1 package in 314ms
Installed 1 package in 15ms
 + black==24.8.0
 
~/uvtmp
❯ ls .venv/lib/python3.12/site-packages/black/*.so
.venv/lib/python3.12/site-packages/black/__init__.cpython-312-darwin.so*	     .venv/lib/python3.12/site-packages/black/mode.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/_width_table.cpython-312-darwin.so*	     .venv/lib/python3.12/site-packages/black/nodes.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/brackets.cpython-312-darwin.so*	     .venv/lib/python3.12/site-packages/black/numerics.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/cache.cpython-312-darwin.so*		     .venv/lib/python3.12/site-packages/black/parsing.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/comments.cpython-312-darwin.so*	     .venv/lib/python3.12/site-packages/black/ranges.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/const.cpython-312-darwin.so*		     .venv/lib/python3.12/site-packages/black/rusty.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/handle_ipynb_magics.cpython-312-darwin.so*  .venv/lib/python3.12/site-packages/black/schema.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/linegen.cpython-312-darwin.so*		     .venv/lib/python3.12/site-packages/black/strings.cpython-312-darwin.so*
.venv/lib/python3.12/site-packages/black/lines.cpython-312-darwin.so*		     .venv/lib/python3.12/site-packages/black/trans.cpython-312-darwin.so*
```

I thought uv was broken, the install with `-v` says `DEBUG Selecting: black==24.8.0 [compatible] (black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl)` and then there aren't any shared object files installed

<details><summary>show full log</summary>

```
~/uvtmp
❯ uv pip install black -v
DEBUG uv 0.4.11 (Homebrew 2024-09-17)
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `redacted/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.5 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: black
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: black*
DEBUG Found fresh response for: https://redacted/packages/pypi/simple/black/
DEBUG Searching for a compatible version of black (*)
DEBUG Selecting: black==24.8.0 [compatible] (black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/77/8d9ce42673e5cb9988f6df73c1c5c1d4e9e788053cccd7f5fb14ef100982/black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for black==24.8.0: click>=8.0.0
DEBUG Adding transitive dependency for black==24.8.0: mypy-extensions>=0.4.3
DEBUG Adding transitive dependency for black==24.8.0: packaging>=22.0
DEBUG Adding transitive dependency for black==24.8.0: pathspec>=0.9.0
DEBUG Adding transitive dependency for black==24.8.0: platformdirs>=2
DEBUG Found fresh response for: https://redacted/packages/pypi/simple/click/
DEBUG Found installed version of click==8.1.7 that satisfies >=8.0.0
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Found installed version of click==8.1.7 that satisfies >=8.0.0
DEBUG Selecting: click==8.1.7 [installed] (installed)
DEBUG Found fresh response for: https://redacted/packages/pypi/simple/mypy-extensions/
DEBUG Found fresh response for: https://redacted/packages/pypi/simple/pathspec/
DEBUG Searching for a compatible version of mypy-extensions (>=0.4.3)
DEBUG Found installed version of mypy-extensions==1.0.0 that satisfies >=0.4.3
DEBUG Selecting: mypy-extensions==1.0.0 [installed] (installed)
DEBUG Found installed version of mypy-extensions==1.0.0 that satisfies >=0.4.3
DEBUG Found installed version of pathspec==0.12.1 that satisfies >=0.9.0
DEBUG Found fresh response for: https://redacted/packages/pypi/simple/packaging/
DEBUG Found fresh response for: https://redacted/packages/pypi/simple/platformdirs/
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Found installed version of packaging==24.1 that satisfies >=22.0
DEBUG Selecting: packaging==24.1 [installed] (installed)
DEBUG Found installed version of packaging==24.1 that satisfies >=22.0
DEBUG Found installed version of platformdirs==4.3.6 that satisfies >=2
DEBUG Searching for a compatible version of pathspec (>=0.9.0)
DEBUG Found installed version of pathspec==0.12.1 that satisfies >=0.9.0
DEBUG Selecting: pathspec==0.12.1 [installed] (installed)
DEBUG Searching for a compatible version of platformdirs (>=2)
DEBUG Found installed version of platformdirs==4.3.6 that satisfies >=2
DEBUG Selecting: platformdirs==4.3.6 [installed] (installed)
DEBUG Tried 6 versions: black 1, click 1, mypy-extensions 1, packaging 1, pathspec 1, platformdirs 1
DEBUG Split specific environment resolution took 0.007s
Resolved 6 packages in 7ms
DEBUG Requirement already cached: black==24.8.0
DEBUG Requirement already installed: click==8.1.7
DEBUG Requirement already installed: mypy-extensions==1.0.0
DEBUG Requirement already installed: packaging==24.1
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: platformdirs==4.3.6
DEBUG Unnecessary package: sqlalchemy==2.0.35
DEBUG Unnecessary package: foo==1.0 (from file://redacted)
DEBUG Preserving seed package: pip==24.2
DEBUG Unnecessary package: typing-extensions==4.12.2
Installed 1 package in 6ms
 + black==24.8.0
DEBUG Released lock at `redacted/.venv/.lock`
```

<details>

---

_Comment by @charliermarsh on 2024-09-18 15:52_

I believe this is documented: https://docs.astral.sh/uv/pip/compatibility/#-no-binary-enforcement

---

_Label `question` added by @charliermarsh on 2024-09-18 15:52_

---

_Comment by @RazerM on 2024-09-18 16:31_

I went directly to https://docs.astral.sh/uv/reference/cli/#uv-pip-install and didn't see anything relevant and never thought to look for the page you mention. It might help if that information is part of the command docs or linked to.

> uv will refuse to install pre-built binary distributions, but will reuse any binary distributions that are already present in the local cache.

Well this is the opposite case but the same logic must apply, I see now.

---

It seems to me that `--no-binary` is not safe without using `--refresh-package` then – I've had packages in the past where the wheels are broken, issues reported upstream, and fixed months later if at all. Our previous solution to this was to set `PIP_NO_BINARY` globally and then all installs on those machines worked fine. Obviously this is not uv's fault but it seems like a usability problem.

---

_Comment by @notatallshaw on 2024-09-18 19:11_

> I believe this is documented: https://docs.astral.sh/uv/pip/compatibility/#-no-binary-enforcement

I'm a little confused how this is different to pip, which will also use a binary cache even when `--no-binary package` is set: https://github.com/pypa/pip/issues/12954

@RazerM Do you not have this same issue with pip then?

---

_Comment by @RazerM on 2024-09-18 22:24_

@notatallshaw That's different. That issue is complaining that a subsequent `--no-binary` install will use whatever was built and cached from the first `--no-binary` install.

pip does not use what it built from `--no-binary` (`black-24.8.0-py3-none-any.whl`) when not using that flag (`black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl`):

```
~/piptemp
❯ pip install --no-binary black black -v
Using pip 24.2 from /redacted/.venv/lib/python3.12/site-packages/pip (python 3.12)
Looking in indexes: https://pypi.python.org/simple
Collecting black
  Using cached black-24.8.0.tar.gz (644 kB)
  Running command pip subprocess to install build dependencies
  Using pip 24.2 from /redacted/.venv/lib/python3.12/site-packages/pip (python 3.12)
  Looking in indexes: https://pypi.python.org/simple
  Collecting hatchling>=1.20.0
    Obtaining dependency information for hatchling>=1.20.0 from https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
    Using cached hatchling-1.25.0-py3-none-any.whl.metadata (3.8 kB)
  Collecting hatch-vcs
    Obtaining dependency information for hatch-vcs from https://files.pythonhosted.org/packages/82/0f/6cbd9976160bc334add63bc2e7a58b1433a31b34b7cda6c5de6dd983d9a7/hatch_vcs-0.4.0-py3-none-any.whl.metadata
    Using cached hatch_vcs-0.4.0-py3-none-any.whl.metadata (8.6 kB)
  Collecting hatch-fancy-pypi-readme
    Obtaining dependency information for hatch-fancy-pypi-readme from https://files.pythonhosted.org/packages/5e/a0/9a7ec6559225e96aa2efb5ab7ef4bf17c42940122a21828c3dce5f62b595/hatch_fancy_pypi_readme-24.1.0-py3-none-any.whl.metadata
    Using cached hatch_fancy_pypi_readme-24.1.0-py3-none-any.whl.metadata (2.0 kB)
  Collecting packaging>=23.2 (from hatchling>=1.20.0)
    Obtaining dependency information for packaging>=23.2 from https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
    Using cached packaging-24.1-py3-none-any.whl.metadata (3.2 kB)
  Collecting pathspec>=0.10.1 (from hatchling>=1.20.0)
    Obtaining dependency information for pathspec>=0.10.1 from https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
    Using cached pathspec-0.12.1-py3-none-any.whl.metadata (21 kB)
  Collecting pluggy>=1.0.0 (from hatchling>=1.20.0)
    Obtaining dependency information for pluggy>=1.0.0 from https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
    Using cached pluggy-1.5.0-py3-none-any.whl.metadata (4.8 kB)
  Collecting trove-classifiers (from hatchling>=1.20.0)
    Obtaining dependency information for trove-classifiers from https://files.pythonhosted.org/packages/b6/7a/e0edec9c8905e851d52076bbc41890603e2ba97cf64966bc1498f2244fd2/trove_classifiers-2024.9.12-py3-none-any.whl.metadata
    Using cached trove_classifiers-2024.9.12-py3-none-any.whl.metadata (2.2 kB)
  Collecting setuptools-scm>=6.4.0 (from hatch-vcs)
    Obtaining dependency information for setuptools-scm>=6.4.0 from https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl.metadata
    Using cached setuptools_scm-8.1.0-py3-none-any.whl.metadata (6.6 kB)
  Collecting setuptools (from setuptools-scm>=6.4.0->hatch-vcs)
    Obtaining dependency information for setuptools from https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl.metadata
    Using cached setuptools-75.1.0-py3-none-any.whl.metadata (6.9 kB)
  Using cached hatchling-1.25.0-py3-none-any.whl (84 kB)
  Using cached hatch_vcs-0.4.0-py3-none-any.whl (8.4 kB)
  Using cached hatch_fancy_pypi_readme-24.1.0-py3-none-any.whl (10 kB)
  Using cached packaging-24.1-py3-none-any.whl (53 kB)
  Using cached pathspec-0.12.1-py3-none-any.whl (31 kB)
  Using cached pluggy-1.5.0-py3-none-any.whl (20 kB)
  Using cached setuptools_scm-8.1.0-py3-none-any.whl (43 kB)
  Using cached trove_classifiers-2024.9.12-py3-none-any.whl (13 kB)
  Using cached setuptools-75.1.0-py3-none-any.whl (1.2 MB)
  Installing collected packages: trove-classifiers, setuptools, pluggy, pathspec, packaging, setuptools-scm, hatchling, hatch-vcs, hatch-fancy-pypi-readme
    Creating /private/var/folders/_p/1cq1n46912n0_4fhfh1l3fkx_r5kw6/T/pip-build-env-1o2ji781/overlay/bin
    changing mode of /private/var/folders/_p/1cq1n46912n0_4fhfh1l3fkx_r5kw6/T/pip-build-env-1o2ji781/overlay/bin/hatchling to 755
    changing mode of /private/var/folders/_p/1cq1n46912n0_4fhfh1l3fkx_r5kw6/T/pip-build-env-1o2ji781/overlay/bin/hatch-fancy-pypi-readme to 755
  Successfully installed hatch-fancy-pypi-readme-24.1.0 hatch-vcs-0.4.0 hatchling-1.25.0 packaging-24.1 pathspec-0.12.1 pluggy-1.5.0 setuptools-75.1.0 setuptools-scm-8.1.0 trove-classifiers-2024.9.12
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  Getting requirements to build wheel ... done
  Running command Preparing metadata (pyproject.toml)
  Preparing metadata (pyproject.toml) ... done
Requirement already satisfied: click>=8.0.0 in /redacted/.venv/lib/python3.12/site-packages (from black) (8.1.7)
Requirement already satisfied: mypy-extensions>=0.4.3 in /redacted/.venv/lib/python3.12/site-packages (from black) (1.0.0)
Requirement already satisfied: packaging>=22.0 in /redacted/.venv/lib/python3.12/site-packages (from black) (24.1)
Requirement already satisfied: pathspec>=0.9.0 in /redacted/.venv/lib/python3.12/site-packages (from black) (0.12.1)
Requirement already satisfied: platformdirs>=2 in /redacted/.venv/lib/python3.12/site-packages (from black) (4.3.6)
Building wheels for collected packages: black
  Running command Building wheel for black (pyproject.toml)
  Building wheel for black (pyproject.toml) ... done
  Created wheel for black: filename=black-24.8.0-py3-none-any.whl size=206504 sha256=972085c618ee94f402da1af548a4f218c754ea7e5dc70acb168bfaca4c2542ed
  Stored in directory: /redacted/Library/Caches/pip/wheels/b7/0f/74/44256d38b1c432c39c1a44805f7005c8a6244704424d9ff33b
Successfully built black
Installing collected packages: black
  changing mode of /redacted/.venv/bin/black to 755
  changing mode of /redacted/.venv/bin/blackd to 755
Successfully installed black-24.8.0

~/piptemp took 3s
❯ ls -l .venv/lib/python3.12/site-packages/black/*.so
zsh: no matches found: .venv/lib/python3.12/site-packages/black/*.so

~/piptemp
❯ pip uninstall -y black
Found existing installation: black 24.8.0
Uninstalling black-24.8.0:
  Successfully uninstalled black-24.8.0

~/piptemp
❯ pip install black -v
Using pip 24.2 from /redacted/.venv/lib/python3.12/site-packages/pip (python 3.12)
Looking in indexes: https://pypi.python.org/simple
Collecting black
  Obtaining dependency information for black from https://files.pythonhosted.org/packages/41/77/8d9ce42673e5cb9988f6df73c1c5c1d4e9e788053cccd7f5fb14ef100982/black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl.metadata
  Using cached black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl.metadata (78 kB)
Requirement already satisfied: click>=8.0.0 in /redacted/.venv/lib/python3.12/site-packages (from black) (8.1.7)
Requirement already satisfied: mypy-extensions>=0.4.3 in /redacted/.venv/lib/python3.12/site-packages (from black) (1.0.0)
Requirement already satisfied: packaging>=22.0 in /redacted/.venv/lib/python3.12/site-packages (from black) (24.1)
Requirement already satisfied: pathspec>=0.9.0 in /redacted/.venv/lib/python3.12/site-packages (from black) (0.12.1)
Requirement already satisfied: platformdirs>=2 in /redacted/.venv/lib/python3.12/site-packages (from black) (4.3.6)
Using cached black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl (1.4 MB)
Installing collected packages: black
  changing mode of /redacted/.venv/bin/black to 755
  changing mode of /redacted/.venv/bin/blackd to 755
Successfully installed black-24.8.0

~/piptemp
❯ ls .venv/lib/python3.12/site-packages/black/*.so
.venv/lib/python3.12/site-packages/black/__init__.cpython-312-darwin.so*       .venv/lib/python3.12/site-packages/black/mode.cpython-312-darwin.so*
...
```

---

_Comment by @charliermarsh on 2024-09-20 19:40_

Re-reading the issue, I don't know if I agree with the premise. If you run `pip install --no-binary black black`, and we build a wheel from source, and then you subsequently run `pip install black`, I think it's completely reasonable to reuse the built wheel.

What I could imagine though is: if the user runs `pip install --no-binary black black`, we build a wheel from source, and then the user runs `pip install --no-build black black`, we probably reuse the built wheel here today, though arguably we shouldn't.


---

_Comment by @zanieb on 2024-09-20 20:11_

>  I don't know if I agree with the premise. If you run `pip install --no-binary black black`, and we build a wheel from source, and then you subsequently run `pip install black`, I think it's completely reasonable to reuse the built wheel.

I agree with this. What about `pip install --no-binary black black` in the second invocation? Are we saying that `--no-binary` and `--no-build` only applies to registry packages?

The `--no-build` case seems weird too. Are you saying you don't want to build `black`? If so, reading from the cache seems fine. Are you're saying you don't want `black` from a source distribution? If so, then I agree we shouldn't reuse the built wheel. It seems like a stretch to say that's the intent of the flag though — you should use `--no-build --no-cache` in that case.

---

_Comment by @RazerM on 2024-09-21 15:29_

> I think it's completely reasonable to reuse the built wheel

I think the logging needs to be better then. There's no indication that uv is using a wheel it built earlier from an sdist. As I said at the end of my post you see the filename of a wheel which is what you expect to be installed:

```
DEBUG Selecting: black==24.8.0 [compatible] (black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/77/8d9ce42673e5cb9988f6df73c1c5c1d4e9e788053cccd7f5fb14ef100982/black-24.8.0-cp312-cp312-macosx_11_0_arm64.whl.metadata
```

Whereas the wheel that was built should look like `black-24.8.0-py3-none-any.whl`.

(I assume uv is just using the logged wheel for metadata, but it's not clear).

I'd like to see the name of the cached wheel that is being used if possible, or maybe just that it's using the cached build of an sdist.

---

_Comment by @KotlinIsland on 2024-10-01 05:49_

> I think the logging needs to be better then.

can i second this? if i run `uv pip install basedmypy --no-binary :all:` it's because i want to debug the source code, so getting a binary anyway is very unintuitive. i just wasted a bunch of time debugging, then thinking i had found a defect in `uv` only to eventually find this issue

perhaps there could be a warning:
```console
> uv pip install basedmypy --no-binary :all:
Resolved 4 packages in 474ms
WARNING: reusing binary edition from cache
Installed 1 package in 3.89s
 + basedmypy==2.6.0
```

additionally, there could be a "work properly" `no-binary` option, that will actually not use a binary.
```console
> uv pip install basedmypy --no-binary :all: --no-binary-force # force???
```

you could just use `--no-cache`, but that's so slow 🐌
```console
> uv pip install basedmypy --no-binary :all: --no-cache
Resolved 4 packages in 524ms
   Built basedmypy==2.6.0
Prepared 1 package in 3m 54s
Installed 1 package in 2.47s
 + basedmypy==2.6.0
 ```

---

_Comment by @zanieb on 2024-10-01 05:55_

Isn't `--no-cache` slow because it's building the package which is what you want? I'm confused.

---

_Comment by @KotlinIsland on 2024-10-01 06:22_

i was referring to the caching of the sdist build, being forced to use `no-cache` to install the `no-binary` version is kinda sucky, but not a show stopper:
```console
> uv pip install basedmypy --no-binary basedmypy
Resolved 4 packages in 686ms
   Built basedmypy==2.6.0
Prepared 1 package in 1m 26s
Installed 1 package in 2.98s
 + basedmypy==2.6.0

> uv pip uninstall basedmypy
Uninstalled 1 package in 2.85s
 - basedmypy==2.6.0

> uv pip install basedmypy --no-binary basedmypy
Resolved 4 packages in 10ms
Installed 1 package in 2.68s
 + basedmypy==2.6.0
 ```

---

_Comment by @KotlinIsland on 2024-10-01 06:26_

although what is alarming to me is that `--no-binary` globally restricts everything to not using a binary, even `--only-binary` doesn't overwrite it:
```py
> uv pip install basedmypy --no-binary basedmypy
... # builds, caches and installs sdist
> cd ../other-project
> uv pip install basedmypy --only-binary basedmypy
... # installs cached sdist

---

_Comment by @charliermarsh on 2024-10-01 12:51_

I think what you're describing is something else, and it was fixed in #7772 (not yet released). You want to avoid using cached registry-binaries when you run with `--no-binary`. This issue is about avoiding cached built wheels when you run _without_ `--no-binary`. They sound similar but they're very different (one is about behavior that's explicit from the user, the other is not).

---

_Closed by @zanieb on 2024-10-21 22:12_

---

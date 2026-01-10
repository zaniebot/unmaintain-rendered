---
number: 8587
title: "BUG: uv pip install gets pre-releases by default (config dependent)"
type: issue
state: closed
author: neutrinoceros
labels:
  - question
assignees: []
created_at: 2024-10-26T07:56:40Z
updated_at: 2024-10-26T16:47:28Z
url: https://github.com/astral-sh/uv/issues/8587
synced_at: 2026-01-10T01:24:30Z
---

# BUG: uv pip install gets pre-releases by default (config dependent)

---

_Issue opened by @neutrinoceros on 2024-10-26 07:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

First, here's my current user level (and only) configuration file
```
❯ cat $HOME/.config/uv/uv.toml
python-preference = 'system'
extra-index-url = ["https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/"]
```
(I'm including the whole file just in case but I don't expect that `python-preference` plays a role here).
For context, https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/ is where fundamental scientific packages like numpy, scipy or pandas upload "nightly" wheels, intended for downstream developers to run tests against bleeding-edge versions without wasting CPU time (and CI complexity) on building them. All wheels up there should be using pre-release version numbers of some sort, so the intent on my side is to go for these wheels when (and *only* when) I pass `--pre` to `uv pip install`.

Here's what happens if I install e.g. numpy in a blank environment, always ignoring cache and varying whether I use `--no-config` or not:

```shell
❯ uv venv
Using CPython 3.13.0 interpreter at: /Users/clm/.pyenv/versions/3.13.0/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

❯ uv pip install numpy --no-cache --no-config 
Resolved 1 package in 3.72s
Prepared 1 package in 5.05s
Installed 1 package in 17ms
 + numpy==2.1.2

❯ uv venv
Using CPython 3.13.0 interpreter at: /Users/clm/.pyenv/versions/3.13.0/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

❯ uv pip install numpy --no-cache
Resolved 1 package in 1.94s
Prepared 1 package in 6.34s
Installed 1 package in 17ms
 + numpy==2.2.0.dev0
```

notice that in the second case, I got a dev version even though I didn't pass `--pre`. I'm almost sure that's a bug, though there could be something I do not understand about how `extra-index-url` is supposed to work.

Here it is again with `--verbose` (assuming a fresh venv again)

```
❯ uv pip install numpy --no-cache --verbose
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/private/tmp/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.0 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: numpy*
DEBUG No cache entry for: https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/numpy/
WARN Skipping file for numpy: numpy.org
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==2.2.0.dev0 [compatible] (numpy-2.2.0.dev0-cp313-cp313-macosx_14_0_arm64.whl)
DEBUG No cache entry for: https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/numpy/2.2.0.dev0/numpy-2.2.0.dev0-cp313-cp313-macosx_14_0_arm64.whl
WARN Range requests not supported for numpy-2.2.0.dev0-cp313-cp313-macosx_14_0_arm64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/numpy/2.2.0.dev0/numpy-2.2.0.dev0-cp313-cp313-macosx_14_0_arm64.whl
DEBUG Tried 1 versions: numpy 1
DEBUG Split specific environment resolution took 1.386s
Resolved 1 package in 1.38s
DEBUG Identified uncached distribution: numpy==2.2.0.dev0
DEBUG No cache entry for: https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/numpy/2.2.0.dev0/numpy-2.2.0.dev0-cp313-cp313-macosx_14_0_arm64.whl
Prepared 1 package in 7.33s
Installed 1 package in 22ms
 + numpy==2.2.0.dev0
DEBUG Released lock at `/private/tmp/.venv/.lock`
```

I note that the current CLI documentation indicates that the corresponding flag is deprecated, though I couldn't figure out how to migrate my configuration file.

```
❯ uv pip install --help | grep 'extra-index-url'
      --extra-index-url <EXTRA_INDEX_URL>          (Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url` [env: UV_EXTRA_INDEX_URL=]
```

I'm running uv 0.4.27 on macOS (Apple silicon) in the all the above. Though I noticed this first on version 0.4.26, I never tried this workflow on an earlier version, so I don't know if it's a regression or not.


---

_Comment by @zanieb on 2024-10-26 16:17_

I think what you're encountering is described in https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes and https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility

The package exists on the first index URL (the extra index you provide) so we do not look at additional indexes. Since there are only pre-release versions available, we accept one without the `--pre` flag. In brief, I think you want to add [`index-strategy = unsafe-best-match`](https://docs.astral.sh/uv/reference/settings/#index-strategy)

---

_Label `question` added by @zanieb on 2024-10-26 16:17_

---

_Comment by @neutrinoceros on 2024-10-26 16:47_

Oh. I'm almost sure I've been bitten by this before, and this is likely to be the *second* issue I open about it 🤦🏻‍♂️ 
Anyway, adding `index-strategy = "unsafe-best-match"` indeed gets me the behavior I intended. Thanks a lot !

---

_Closed by @neutrinoceros on 2024-10-26 16:47_

---

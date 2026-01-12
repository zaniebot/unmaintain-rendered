```yaml
number: 4766
title: "Divergence between `uv pip install` and `pip install` with pre-releases"
type: issue
state: closed
author: Peiffap
labels: []
assignees: []
created_at: 2024-07-03T12:22:08Z
updated_at: 2024-07-03T12:50:11Z
url: https://github.com/astral-sh/uv/issues/4766
synced_at: 2026-01-12T15:58:52Z
```

# Divergence between `uv pip install` and `pip install` with pre-releases

---

_@Peiffap_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

While trying to debug https://github.com/networkx/networkx/pull/7541, I ran into the following divergence between `pip install --pre` and `uv pip install --prerelease=allow`. Here is my terminal output for both.

```sh
networkx on  main [!?] via  v3.12.4
❯ python --version
Python 3.12.4

networkx on  main [!?] via  v3.12.4
❯ uv --version
uv 0.2.21 (ebfe6d8fc 2024-07-03)

networkx on  main [!?] via  v3.12.4
❯ uv venv nx-devdeps
Using Python 3.12.4 interpreter at: /usr/local/opt/python@3.12/bin/python3.12
Creating virtualenv at: nx-devdeps
Activate with: source nx-devdeps/bin/activate

networkx on  main [!?] via  v3.12.4
❯ source nx-devdeps/bin/activate

networkx on  main [!?] via  v3.12.4 (nx-devdeps)
❯ uv pip install -U --prerelease=allow --extra-index-url https://pypi.org/simple -i https://pypi.anaconda.org/scientific-python-nightly-wheels/simple matplotlib

Resolved 11 packages in 29ms
Installed 10 packages in 59ms
 + contourpy==1.2.1
 + cycler==0.12.1
 + fonttools==4.53.0
 + kiwisolver==1.4.5
 + matplotlib==3.9.0
 + packaging==24.1
 + pillow==10.4.0
 + pyparsing==3.1.2
 + python-dateutil==2.9.0.post0
 + six==1.16.0
```

```sh
networkx on  main [!?] via  v3.12.4
❯ python -m venv nx-devdeps

networkx on  main [!?] via  v3.12.4 took 6s
❯ source nx-devdeps/bin/activate

networkx on  main [!?] via  v3.12.4 (nx-devdeps)
❯ pip install -U --prerelease --extra-index-url https://pypi.org/simple -i https://pypi.anaconda.org/scientific-python-nightly-wheels/simple matplotlib


Usage:
  pip install [options] <requirement specifier> [package-index-options] ...
  pip install [options] -r <requirements file> [package-index-options] ...
  pip install [options] [-e] <vcs project url> ...
  pip install [options] [-e] <local project path> ...
  pip install [options] <archive url/path> ...

no such option: --prerelease

networkx on  main [!?] via  v3.12.4 (nx-devdeps)
❯ pip install -U --pre --extra-index-url https://pypi.org/simple -i https://pypi.anaconda.org/scientific-python-nightly-wheels/simple matplotlib

Looking in indexes: https://pypi.anaconda.org/scientific-python-nightly-wheels/simple, https://pypi.org/simple
WARNING: Cache entry deserialization failed, entry ignored
Collecting matplotlib
  Downloading https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/matplotlib/3.10.0.dev344%2Bg651e9109e9/matplotlib-3.10.0.dev344%2Bg651e9109e9-cp312-cp312-macosx_10_12_x86_64.whl (7.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.9/7.9 MB 8.0 MB/s eta 0:00:00
WARNING: Cache entry deserialization failed, entry ignored
Collecting contourpy>=1.0.1 (from matplotlib)
  Downloading https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/contourpy/1.3.0.dev1/contourpy-1.3.0.dev1-cp312-cp312-macosx_10_9_x86_64.whl (267 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 267.8/267.8 kB 5.7 MB/s eta 0:00:00
Collecting cycler>=0.10 (from matplotlib)
  Using cached cycler-0.12.1-py3-none-any.whl.metadata (3.8 kB)
Collecting fonttools>=4.22.0 (from matplotlib)
  Using cached fonttools-4.53.0-cp312-cp312-macosx_10_9_universal2.whl.metadata (162 kB)
Collecting kiwisolver>=1.3.1 (from matplotlib)
  Using cached kiwisolver-1.4.5-cp312-cp312-macosx_10_9_x86_64.whl.metadata (6.4 kB)
WARNING: Cache entry deserialization failed, entry ignored
Collecting numpy>=1.23 (from matplotlib)
  Downloading https://pypi.anaconda.org/scientific-python-nightly-wheels/simple/numpy/2.1.0.dev0/numpy-2.1.0.dev0-cp312-cp312-macosx_10_9_x86_64.whl (20.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 20.9/20.9 MB 8.7 MB/s eta 0:00:00
Collecting packaging>=20.0 (from matplotlib)
  Using cached packaging-24.1-py3-none-any.whl.metadata (3.2 kB)
Collecting pillow>=8 (from matplotlib)
  Using cached pillow-10.4.0-cp312-cp312-macosx_10_10_x86_64.whl.metadata (9.2 kB)
Collecting pyparsing>=2.3.1 (from matplotlib)
  Using cached pyparsing-3.1.2-py3-none-any.whl.metadata (5.1 kB)
Collecting python-dateutil>=2.7 (from matplotlib)
  Using cached python_dateutil-2.9.0.post0-py2.py3-none-any.whl.metadata (8.4 kB)
Collecting six>=1.5 (from python-dateutil>=2.7->matplotlib)
  Using cached six-1.16.0-py2.py3-none-any.whl.metadata (1.8 kB)
Using cached cycler-0.12.1-py3-none-any.whl (8.3 kB)
Using cached fonttools-4.53.0-cp312-cp312-macosx_10_9_universal2.whl (2.8 MB)
Using cached kiwisolver-1.4.5-cp312-cp312-macosx_10_9_x86_64.whl (67 kB)
Using cached packaging-24.1-py3-none-any.whl (53 kB)
Using cached pillow-10.4.0-cp312-cp312-macosx_10_10_x86_64.whl (3.5 MB)
Using cached pyparsing-3.1.2-py3-none-any.whl (103 kB)
Using cached python_dateutil-2.9.0.post0-py2.py3-none-any.whl (229 kB)
Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Installing collected packages: six, pyparsing, pillow, packaging, numpy, kiwisolver, fonttools, cycler, python-dateutil, contourpy, matplotlib
Successfully installed contourpy-1.3.0.dev1 cycler-0.12.1 fonttools-4.53.0 kiwisolver-1.4.5 matplotlib-3.10.0.dev344+g651e9109e9 numpy-2.1.0.dev0 packaging-24.1 pillow-10.4.0 pyparsing-3.1.2 python-dateutil-2.9.0.post0 six-1.16.0

[notice] A new release of pip is available: 24.0 -> 24.1.1
[notice] To update, run: pip install --upgrade pip

networkx on  main [!?] via  v3.12.4 (nx-devdeps) took 23s
❯ pip list
Package         Version
--------------- -------------------------
contourpy       1.3.0.dev1
cycler          0.12.1
fonttools       4.53.0
kiwisolver      1.4.5
matplotlib      3.10.0.dev344+g651e9109e9
numpy           2.1.0.dev0
packaging       24.1
pillow          10.4.0
pip             24.0
pyparsing       3.1.2
python-dateutil 2.9.0.post0
six             1.16.0

[notice] A new release of pip is available: 24.0 -> 24.1.1
[notice] To update, run: pip install --upgrade pip
```

Note the differing versions of `contourpy`, for example. I know prereleases are [difficult to work with](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#pre-release-compatibility), but my understanding was that `--prerelease=allow` should make this install the latest possible prereleases... Apologies if I misunderstood!

Edit: I'm on macOS 12.7.5.

---

_Comment by @charliermarsh on 2024-07-03 12:23_

Can you try inverting the order of your index URLs? So making the Conda index the `--extra-index-url`?

---

_Comment by @Peiffap on 2024-07-03 12:28_

That does seem to fix things... Can you explain why? Am I doing something wrong or is this a design choice?

```sh
networkx on  main [!?] via  v3.12.4
❯ uv venv nx-devdeps
Using Python 3.12.4 interpreter at: /usr/local/opt/python@3.12/bin/python3.12
Creating virtualenv at: nx-devdeps
Activate with: source nx-devdeps/bin/activate

networkx on  main [!?] via  v3.12.4
❯ source nx-devdeps/bin/activate

networkx on  main [!?] via  v3.12.4 (nx-devdeps)
❯ uv pip install -U --prerelease=allow --extra-index-url https://pypi.anaconda.org/scientific-python-nightly-wheels/simple -i https://pypi.org/simple matplotlib
Resolved 11 packages in 6.07s
Prepared 3 packages in 3.86s
Installed 11 packages in 148ms
 + contourpy==1.3.0.dev1
 + cycler==0.12.1
 + fonttools==4.53.0
 + kiwisolver==1.4.5
 + matplotlib==3.10.0.dev344+g651e9109e9
 + numpy==2.1.0.dev0
 + packaging==24.1
 + pillow==10.4.0
 + pyparsing==3.1.2
 + python-dateutil==2.9.0.post0
 + six==1.16.0
```

I'm also genuinely _amazed_ at how fast you got to this... Love what you guys are doing at Astral!

---

_Comment by @Peiffap on 2024-07-03 12:29_

Nevermind, I think I found it
> When uv searches for a package across multiple indexes, it will iterate over the indexes in order (preferring the --extra-index-url over the default index), and stop searching as soon as it finds a match. This means that if a package exists on multiple indexes, uv will limit its candidate versions to those present in the first index that contains the package.

Basically the nightlies are only on Conda, not PyPI, and when the version on PyPI got found, uv stopped resolving. (Right?)

---

_Comment by @charliermarsh on 2024-07-03 12:30_

(Started writing this before you responded so I'll post it anyway but yes, you're right.)

We have different default behavior when looking at multiple indexes -- we settled on something that we think is safer by default: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#packages-that-exist-on-multiple-indexes. So if a package exists on multiple indexes, we stop looking as soon as we find the _first_ index that contains it (preferring `--extra-index-url` over `--index-url`) to avoid confusion attacks.

You can set `--index-strategy unsafe-best-match` which I _think_ would give you the same resolution without swapping the order (it's meant to mirror the pip default more closely).


---

_Closed by @Peiffap on 2024-07-03 12:31_

---

_Comment by @Peiffap on 2024-07-03 12:31_

That's great and makes a lot of sense. Thanks!

---

_Comment by @charliermarsh on 2024-07-03 12:50_

No prob!

---

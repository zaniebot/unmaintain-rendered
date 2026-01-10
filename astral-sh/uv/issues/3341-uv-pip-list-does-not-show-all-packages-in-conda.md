---
number: 3341
title: uv pip list does not show all packages in conda environment
type: issue
state: closed
author: gboeing
labels: []
assignees: []
created_at: 2024-05-03T03:40:12Z
updated_at: 2024-05-06T13:47:29Z
url: https://github.com/astral-sh/uv/issues/3341
synced_at: 2026-01-10T01:23:27Z
---

# uv pip list does not show all packages in conda environment

---

_Issue opened by @gboeing on 2024-05-03 03:40_

This seems to be related to, but different from, #2841 and #3164. I'm using 0.1.39 on Ubuntu.

If I create a conda environment and activate it, `pip list` and `uv pip list` show different packages installed in the environment.

```
conda create -n TEST pip setuptools uv zstandard
conda activate TEST
pip show zstandard
  # Name: zstandard
  # Version: 0.22.0
  # Summary: Zstandard bindings for Python
  # Home-page: https://github.com/indygreg/python-zstandard
  # Author: Gregory Szorc
  # Author-email: gregory.szorc@gmail.com
  # License: BSD
  # Location: /home/user/mambaforge/envs/TEST/lib/python3.12/site-packages
  # Requires: 
  # Required-by: 
uv pip show zstandard
  # warning: Package(s) not found for: zstandard
pip show setuptools
  # Name: setuptools
  # Version: 69.5.1
  # Summary: Easily download, build, install, upgrade, and uninstall Python packages
  # Home-page: https://github.com/pypa/setuptools
  # Author: Python Packaging Authority
  # Author-email: distutils-sig@python.org
  # License: 
  # Location: /home/user/mambaforge/envs/TEST/lib/python3.12/site-packages
  # Requires: 
  # Required-by: 
uv pip show setuptools
  # warning: Package(s) not found for: setuptools
pip list
  # Package    Version
  # ---------- -------
  # cffi       1.16.0
  # pip        24.0
  # pycparser  2.22
  # setuptools 69.5.1
  # wheel      0.43.0
  # zstandard  0.22.0
uv pip list
  # Package   Version
  # --------- -------
  # cffi      1.16.0
  # pycparser 2.22
  # wheel     0.43.0
```

So, within the TEST environment, pip can see (for example) setuptools and zstandard, but uv cannot.

---

_Comment by @gboeing on 2024-05-03 15:04_

Another example. If I run:

```
conda create -y -n TEST hatch pip uv
conda activate TEST
uv pip list --strict
```

I get three warnings:

```
warning: The package `hatch` requires `uv>=0.1.35`, but it's not installed.
warning: The package `hatch` requires `zstandard<1`, but it's not installed.
warning: The package `h2` requires `hpack<5,>=4.0`, but it's not installed.
```

But I can see these are installed in the environment:

```
which uv
  # /home/user/miniforge3/envs/TEST/bin/uv
pip list -v
  # Package             Version   Location                                                      Installer
  # ------------------- --------- ------------------------------------------------------------- ---------
  # hatch               1.10.0    /home/user/miniforge3/envs/TEST/lib/python3.12/site-packages  conda
  # hpack               4.0.0     /home/user/miniforge3/envs/TEST/lib/python3.12/site-packages
  # zstandard           0.22.0    /home/user/miniforge3/envs/TEST/lib/python3.12/site-packages
  # (other rows omitted for brevity)
```

My only clue that comes out of this is that `hpack` and `zstandard` do not show anything in the "Installer" column when running `pip list -v`, and those are two of the packages the warning says are missing in the environment though they are installed.

---

_Comment by @charliermarsh on 2024-05-03 15:08_

I believe this is the same underlying issue as https://github.com/astral-sh/uv/issues/2500, which is (I'm guessing) those packages are located somewhere else in `sys.path`, but we're limiting our discovery to the environment's site-packages.

---

_Comment by @gboeing on 2024-05-03 17:06_

Thanks @charliermarsh. 

> I believe this is the same underlying issue as https://github.com/astral-sh/uv/issues/2500, which is (I'm guessing) those packages are located somewhere else in sys.path, but we're limiting our discovery to the environment's site-packages.

Even if they are *also* located somewhere else in `sys.path`, you can see in this [comment](https://github.com/astral-sh/uv/issues/3341#issuecomment-2093204691) that they are in fact in this environment's site-packages. So, if `uv` limits its discovery to the environment's site-packages, they should be listed, right?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-03 17:08_

---

_Comment by @charliermarsh on 2024-05-03 17:08_

I'll take a look when I have a sec.

---

_Comment by @samypr100 on 2024-05-03 20:26_

@gboeing Could you list out the raw contents of the site-packages dir?
e.g. via something like `ls -l /home/user/mambaforge/envs/TEST/lib/python3.12/site-packages`

I wonder if there's a possibility they're installed as eggs.

---

_Comment by @gboeing on 2024-05-03 20:35_

@samypr100 here you go:
```
total 776
drwxrwxr-x  7 geoff geoff   4096 May  3 13:33 anyio
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 anyio-4.3.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 backports
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 backports.tarfile-1.0.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 certifi
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 certifi-2024.2.2.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 cffi
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 cffi-1.16.0.dist-info
-rwxrwxr-x  3 geoff geoff 219480 Sep 29  2023 _cffi_backend.cpython-312-x86_64-linux-gnu.so
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 click
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 click-8.1.7.dist-info
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 cryptography
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 cryptography-42.0.5.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 distlib
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 distlib-0.3.8.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 _distutils_hack
-rw-rw-r--  4 geoff geoff    151 Apr 14 04:42 distutils-precedence.pth
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 editables
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 editables-0.5.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 exceptiongroup
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 exceptiongroup-1.2.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 filelock
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 filelock-3.14.0.dist-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 h11
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 h11-0.14.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 h2
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 h2-4.1.0.dist-info
drwxrwxr-x 15 geoff geoff   4096 May  3 13:33 hatch
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 hatch-1.10.0.dist-info
drwxrwxr-x 12 geoff geoff   4096 May  3 13:33 hatchling
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 hatchling-1.24.2.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 hpack
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 hpack-4.0.0-py3.6.egg-info
drwxrwxr-x  6 geoff geoff   4096 May  3 13:33 httpcore
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 httpcore-1.0.5.dist-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 httpx
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 httpx-0.27.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 hyperframe
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 hyperframe-6.0.1.dist-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 hyperlink
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 hyperlink-21.0.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 idna
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 idna-3.7.dist-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 importlib_metadata
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 importlib_metadata-7.1.0.dist-info
drwxrwxr-x  6 geoff geoff   4096 May  3 13:33 importlib_resources
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 importlib_resources-6.4.0.dist-info
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 jaraco
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 jaraco.classes-3.4.0.dist-info
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 jaraco.context-5.3.0.dist-info
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 jaraco.functools-4.0.0.dist-info
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 jeepney
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 jeepney-0.8.0.dist-info
drwxrwxr-x  7 geoff geoff   4096 May  3 13:33 keyring
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 keyring-25.2.0.dist-info
drwxrwxr-x 10 geoff geoff   4096 May  3 13:33 markdown_it
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 markdown_it_py-3.0.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 mdurl
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 mdurl-0.1.2.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 more_itertools
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 more_itertools-10.2.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 packaging
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 packaging-24.0.dist-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 pathspec
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 pathspec-0.12.1.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 pexpect
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 pexpect-4.9.0.dist-info
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 pip
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 pip-24.0-py3.12.egg-info
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 pkg_resources
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 platformdirs
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 platformdirs-4.2.1.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 pluggy
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 pluggy-1.5.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 ptyprocess
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 ptyprocess-0.7.0.dist-info
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 __pycache__
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 pycparser
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 pycparser-2.22.dist-info
drwxrwxr-x  7 geoff geoff   4096 May  3 13:33 pygments
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 pygments-2.17.2.dist-info
-rw-rw-r--  3 geoff geoff    119 Apr 15 12:06 README.txt
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 rich
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 rich-13.7.1.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 secretstorage
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 SecretStorage-3.3.3.dist-info
drwxrwxr-x  9 geoff geoff   4096 May  3 13:33 setuptools
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 setuptools-69.5.1-py3.12.egg-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 shellingham
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 shellingham-1.5.4.dist-info
drwxrwxr-x  4 geoff geoff   4096 May  3 13:33 sniffio
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 sniffio-1.3.1.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 tomli
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 tomli-2.0.1.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 tomli_w
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 tomli_w-1.0.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 tomlkit
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 tomlkit-0.12.4.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 trove_classifiers
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 trove_classifiers-2024.4.10.dist-info
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 typing_extensions-4.11.0.dist-info
-rw-rw-r--  3 geoff geoff 122293 Apr  5 08:14 typing_extensions.py
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 userpath
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 userpath-1.7.0.dist-info
drwxrwxr-x 11 geoff geoff   4096 May  3 13:33 virtualenv
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 virtualenv-20.26.1.dist-info
drwxrwxr-x  5 geoff geoff   4096 May  3 13:33 wheel
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 wheel-0.43.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 zipp
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 zipp-3.17.0.dist-info
drwxrwxr-x  3 geoff geoff   4096 May  3 13:33 zstandard
drwxrwxr-x  2 geoff geoff   4096 May  3 13:33 zstandard-0.22.0-py3.12.egg-info
```

---

_Comment by @charliermarsh on 2024-05-03 21:02_

Ah ok, yeah, we don't support egg-info files.

---

_Comment by @charliermarsh on 2024-05-03 21:09_

@hauntsaninja -- am I right that what you requested the other day was ability to uninstall `.egg-info`?

---

_Comment by @samypr100 on 2024-05-03 22:06_

Crosslinking to https://github.com/astral-sh/uv/issues/2928

---

_Comment by @hauntsaninja on 2024-05-03 22:11_

Yeah, if `uv` supported uninstalling what pip calls "legacy editable" `.egg-info` that would be useful for us. Specifically we would find it useful if uv could do this: https://github.com/pypa/pip/blob/41587f5e0017bcd849f42b314dc8a34a7db75621/src/pip/_internal/req/req_uninstall.py#L534-L552 (e.g. during `uv pip sync`)
Note I think "legacy editable" is a slightly different case than what's going on^^

This is getting a little off topic, but to clarify my exact request... while I certainly won't complain if you do, I'm perfectly happy for uv to not support legacy editables. The thing that I am very intrigued by is reducing the cost of editable installations going forward. Currently, with PEP 660, every editable installation creates a new `.pth` file (and depending on the build backend, what that `.pth` file does might actually be quite expensive). But even just the presence of `.pth` files slows down Python interpreter startup.

I just checked and in my work environment I have ~400 projects editably installed. With the legacy editable installation, these are all in `easy-install.pth`. If instead I write these one line per file, Python interpreter startup is much slower, even with warm disk cache:
```

(v1) ~/tmp/l/v1/.venv/lib/python3.12/site-packages λ ls -1 *.pth | wc -l                     
       2
(v1) ~/tmp/l/v1/.venv/lib/python3.12/site-packages λ cat easy-install.pth | wc -l
     386

(v2) ~/tmp/l/v2/.venv/lib/python3.12/site-packages λ ls -1 *.pth | wc -l
     387

λ hyperfine -w 1 'v1/.venv/bin/python -c pass' 'v2/.venv/bin/python -c pass'
Benchmark 1: v1/.venv/bin/python -c pass
  Time (mean ± σ):      36.7 ms ±   9.8 ms    [User: 13.2 ms, System: 6.2 ms]
  Range (min … max):    25.3 ms …  66.5 ms    79 runs
 
Benchmark 2: v2/.venv/bin/python -c pass
  Time (mean ± σ):      91.2 ms ±   3.5 ms    [User: 23.8 ms, System: 27.8 ms]
  Range (min … max):    86.6 ms … 101.3 ms    30 runs
 
Summary
  v1/.venv/bin/python -c pass ran
    2.49 ± 0.67 times faster than v2/.venv/bin/python -c pass
```
In general, I think there's opportunities for doing really nice end-to-end optimisation for editable setups if uv were to have its own build backend (e.g. installation can be instant).



---

_Comment by @charliermarsh on 2024-05-03 22:34_

@hauntsaninja - thank you, exactly the info I was looking for (and understood that this is slightly different than the linked issue).


---

_Comment by @charliermarsh on 2024-05-03 22:34_

I'm gonna merge this into https://github.com/astral-sh/uv/issues/2928.

---

_Closed by @charliermarsh on 2024-05-03 22:34_

---

_Comment by @zanieb on 2024-05-03 23:15_

cc @konstin regarding the workspace editables.

---

_Referenced in [astral-sh/uv#3380](../../astral-sh/uv/pulls/3380.md) on 2024-05-05 23:07_

---

_Comment by @charliermarsh on 2024-05-05 23:21_

Put up a PR to fix this: https://github.com/astral-sh/uv/pull/3380

---

_Closed by @charliermarsh on 2024-05-06 13:47_

---

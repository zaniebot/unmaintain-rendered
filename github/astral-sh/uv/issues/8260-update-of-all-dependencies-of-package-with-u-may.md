---
number: 8260
title: "Update of all dependencies of package  with `-U` may lead to not working environment (`numba` and `numpy` problem)"
type: issue
state: closed
author: Czaki
labels: []
assignees: []
created_at: 2024-10-16T14:59:31Z
updated_at: 2024-10-16T17:23:16Z
url: https://github.com/astral-sh/uv/issues/8260
synced_at: 2026-01-07T13:12:17-06:00
---

# Update of all dependencies of package  with `-U` may lead to not working environment (`numba` and `numpy` problem)

---

_Issue opened by @Czaki on 2024-10-16 14:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When updated, a package using `uv pip install -U` the `uv` updates its dependencies (which is nice), but it ignores upper constraints in dependencies of all installed packages. 

In contradiction, the `pip` update only these dependencies that are enforced by minimum required version of dependency. 

In my case, it creates a problem with any package that has `numpy` in dependencies as `numba` project do not support the latest `numpy` version. 

Here MRE (in real scenario I was switching the `zarr` version). 

```bash
$ uv pip install numba
Using Python 3.11.10 environment at venv
Resolved 3 packages in 108ms
Prepared 2 packages in 1.53s
Installed 3 packages in 48ms
 + llvmlite==0.43.0
 + numba==0.60.0
 + numpy==2.0.2

$ uv pip install zarr
Using Python 3.11.10 environment at venv
Resolved 5 packages in 5ms
Installed 4 packages in 1ms
 + asciitree==0.3.3
 + fasteners==0.19
 + numcodecs==0.13.1
 + zarr==2.18.3

$ uv pip install -U zarr 
Using Python 3.11.10 environment at venv
Resolved 5 packages in 124ms
Prepared 1 package in 0.58ms
Uninstalled 1 package in 14ms
Installed 1 package in 16ms
 - numpy==2.0.2
 + numpy==2.1.2

$ python                                                                                                                                  
Python 3.11.10 (main, Oct  2 2024, 16:32:22) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numba
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/czaki/venv/lib/python3.11/site-packages/numba/__init__.py", line 59, in <module>
    _ensure_critical_deps()
  File "/home/czaki/venv/lib/python3.11/site-packages/numba/__init__.py", line 45, in _ensure_critical_deps
    raise ImportError(msg)
ImportError: Numba needs NumPy 2.0 or less. Got NumPy 2.1.
```

When call update of both `numba` and `zarr` it selects the correct NumPy version

```bash
$ uv pip install -U zarr numba
Using Python 3.11.10 environment at venv
Resolved 7 packages in 156ms
Prepared 1 package in 0.64ms
Uninstalled 1 package in 18ms
Installed 1 package in 13ms
 - numpy==2.1.2
 + numpy==2.0.2
```

Platform: Ubuntu 24.04 
`uv` version: 0.4.22

It will be nice if `uv` could take a care of upper constraints of already installed package and require `--override` flag to ignore them. 

---

_Comment by @charliermarsh on 2024-10-16 15:02_

Just to clarify what's going on here, I don't think pip is taking Numba into account at all. Rather, I think pip treats `pip install -U zarr` as "upgrade zarr", whereas we treat `pip install -U zarr` as "upgrade any dependencies you encounter when resolving zarr".

---

_Comment by @charliermarsh on 2024-10-16 15:03_

pip will also let you break the environment trivially (`pip install numpy==2.1.2` would have the same problem).

---

_Comment by @Czaki on 2024-10-16 15:13_

Yes. I agree. There is difference, I try to point it in description.

But with pip I end with working environment, and with `uv`, the environment is broken. 

I also expect that `uv pip install numpy==2.1.2` will break the environment. I even expect that `uv pip install -U numpy` will do this. 

But now, I do not know how to update a single package in a way that does not break the environment, without going on PyPI and manual typing version in terminal. 

---

_Comment by @charliermarsh on 2024-10-16 15:15_

If you only want to upgrade `zarr`, you can do `uv pip install --upgrade-package zarr zarr`

---

_Comment by @charliermarsh on 2024-10-16 16:34_

I think there's some overlap with: https://github.com/astral-sh/uv/issues/4779

---

_Comment by @charliermarsh on 2024-10-16 17:22_

Oh, and also: https://github.com/astral-sh/uv/issues/6100

---

_Comment by @charliermarsh on 2024-10-16 17:23_

I'm going to merge into https://github.com/astral-sh/uv/issues/4779 so we have a single issue for this.

---

_Closed by @charliermarsh on 2024-10-16 17:23_

---

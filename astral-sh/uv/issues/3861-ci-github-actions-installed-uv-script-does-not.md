---
number: 3861
title: "CI (Github Actions): Installed uv script does not detect any Python interpreters"
type: issue
state: closed
author: jenisys
labels:
  - error messages
assignees: []
created_at: 2024-05-27T09:14:15Z
updated_at: 2024-05-28T13:13:09Z
url: https://github.com/astral-sh/uv/issues/3861
synced_at: 2026-01-10T01:23:31Z
---

# CI (Github Actions): Installed uv script does not detect any Python interpreters

---

_Issue opened by @jenisys on 2024-05-27 09:14_

I try using `uv` to speed up Python package installations in the CI (concrete: Github Actions).


## SYNDROME:

When I use the following `workflow` snippet, I run into a problem that `uv` cannot find any Python interpreters.

```yaml
# -- CI WORKFLOW SNIPPET: With Python3
  ...
  - name: "Install Python package dependencies (with: uv)"
    run: |
      python -m pip install -U uv
      # -- FAILURE-POINT: On next line
      uv pip install -U pip setuptools wheel 
```

Running Github Actions CI causes errors on `ubuntu-latest` (hint: `windows` works fine):

```
...
Installing collected packages: uv
Successfully installed uv-0.2.4
error: No Python interpreters found in virtual environments
Error: Process completed with exit code 2.
```

SEE ALSO:
* https://github.com/jenisys/parse_type/actions/runs/9252100544


## WORK-AROUND

Use `uv` as python module to run `uv`, like:

```yaml
# -- CI WORKFLOW SNIPPET:
  - name: "Install Python package dependencies (with: uv)"
    run: |
      python -m pip install -U uv
      python -m uv pip install -U pip setuptools wheel
```

---

_Label `error messages` added by @konstin on 2024-05-27 09:32_

---

_Comment by @konstin on 2024-05-27 09:51_

`uv pip install --system` should also work for you (https://github.com/konstin/parse_type/actions/runs/9252778300/job/25451090432?pr=1). Improving the confusing error message here is covered by https://github.com/astral-sh/uv/issues/3841

---

_Comment by @charliermarsh on 2024-05-27 14:22_

Yeah you need the system flag. By default, uv only installs into virtual environments.

---

_Closed by @charliermarsh on 2024-05-27 14:22_

---

_Comment by @korhojoa on 2024-05-27 15:35_

I get this even if using a virtual environment.

Calling uv via `bin/uv` fails, but using `bin/python -m uv` works.

```
$ make devel
python3 -m venv venv
cp pip.conf venv/
venv/bin/python -m pip install --upgrade pip setuptools setuptools_scm wheel build uv
Looking in indexes: <snip>
Requirement already satisfied: pip in ./venv/lib/python3.11/site-packages (24.0)
Requirement already satisfied: setuptools in ./venv/lib/python3.11/site-packages (65.5.0)
Collecting setuptools
  Using cached <snip>/setuptools-70.0.0-py3-none-any.whl (863 kB)
Collecting setuptools_scm
  Using cached <snip>/setuptools_scm-8.1.0-py3-none-any.whl (43 kB)
Collecting wheel
  Using cached <snip>/wheel-0.43.0-py3-none-any.whl (65 kB)
Collecting build
  Using cached <snip>/build-1.2.1-py3-none-any.whl (21 kB)
Collecting uv
  Using cached <snip>/uv-0.2.4-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.9 MB)
Collecting packaging>=20 (from setuptools_scm)
  Using cached <snip>/packaging-24.0-py3-none-any.whl (53 kB)
Collecting pyproject_hooks (from build)
  Using cached <snip>/pyproject_hooks-1.1.0-py3-none-any.whl (9.2 kB)
Installing collected packages: wheel, uv, setuptools, pyproject_hooks, packaging, setuptools_scm, build
  Attempting uninstall: setuptools
    Found existing installation: setuptools 65.5.0
    Uninstalling setuptools-65.5.0:
      Successfully uninstalled setuptools-65.5.0
Successfully installed build-1.2.1 packaging-24.0 pyproject_hooks-1.1.0 setuptools-70.0.0 setuptools_scm-8.1.0 uv-0.2.4 wheel-0.43.0
touch venv/bin/activate
venv/bin/uv  pip install --upgrade pip-tools
error: No Python interpreters found in virtual environments
make: *** [makefile:87: venv/bin/pip-sync] Error 2
```
versus calling the via the module
```
$ make devel
python3 -m venv venv
cp pip.conf venv/
venv/bin/python -m pip install --upgrade pip setuptools setuptools_scm wheel build uv
Looking in indexes: <snip>
Requirement already satisfied: pip in ./venv/lib/python3.11/site-packages (24.0)
Requirement already satisfied: setuptools in ./venv/lib/python3.11/site-packages (65.5.0)
Collecting setuptools
  Using cached <snip>/setuptools-70.0.0-py3-none-any.whl (863 kB)
Collecting setuptools_scm
  Using cached <snip>/setuptools_scm-8.1.0-py3-none-any.whl (43 kB)
Collecting wheel
  Using cached <snip>/wheel-0.43.0-py3-none-any.whl (65 kB)
Collecting build
  Using cached <snip>/build-1.2.1-py3-none-any.whl (21 kB)
Collecting uv
  Using cached <snip>/uv-0.2.4-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.9 MB)
Collecting packaging>=20 (from setuptools_scm)
  Using cached <snip>/packaging-24.0-py3-none-any.whl (53 kB)
Collecting pyproject_hooks (from build)
  Using cached <snip>/pyproject_hooks-1.1.0-py3-none-any.whl (9.2 kB)
Installing collected packages: wheel, uv, setuptools, pyproject_hooks, packaging, setuptools_scm, build
  Attempting uninstall: setuptools
    Found existing installation: setuptools 65.5.0
    Uninstalling setuptools-65.5.0:
      Successfully uninstalled setuptools-65.5.0
Successfully installed build-1.2.1 packaging-24.0 pyproject_hooks-1.1.0 setuptools-70.0.0 setuptools_scm-8.1.0 uv-0.2.4 wheel-0.43.0
touch venv/bin/activate
venv/bin/python -m uv    pip install --upgrade pip-tools
Resolved 8 packages in 795ms
Installed 2 packages in 4ms
 + click==8.1.7
 + pip-tools==7.4.1
touch venv/bin/pip-sync
venv/bin/python -m uv    pip sync requirements.txt requirements-test.txt
Resolved 55 packages in 5.81s
Uninstalled 5 packages in 6ms
Installed 54 packages in 57ms
...
```

---

_Comment by @charliermarsh on 2024-05-27 16:31_

You need to activate the virtual environment above -- you seem to have `touch venv/bin/activate`, but that should be `source venv/bin/activate`, right?

---

_Comment by @korhojoa on 2024-05-28 04:58_

> You need to activate the virtual environment above -- you seem to have `touch venv/bin/activate`, but that should be `source venv/bin/activate`, right?

This is intentional, the touch is done for makefile rule purposes, it sets the last modified time of the file so the make target doesn't need to run. We don't `source venv/bin/activate` when running commands in the venv.

Tools/binaries/scripts can be run directly (at least according to [documentation](https://docs.python.org/3/library/venv.html)) by specifying their path within the venv. I understand that as it's a binary, there's no shebang to add/modify, so probably some handling of this case is necessary unless this is something where drop-in compatibility is not wished for.

Example:
```
$ python3 -m venv demo
$ demo/bin/pip install uv
Looking in indexes: <snip>
Collecting uv
  Using cached <snip>/uv-0.2.4-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.9 MB)
Installing collected packages: uv
Successfully installed uv-0.2.4
$ demo/bin/uv pip list
Package    Version
---------- -------
pip        24.0
setuptools 65.5.0
uv         0.2.4
$ demo/bin/uv pip install -U uv
error: No Python interpreters found in virtual environments
$ demo/bin/python -m uv pip install -U uv
⠼ Resolving dependencies...
^C
```
edit: specifying python manually also works:
```
$ UV_PYTHON=demo/bin/python demo/bin/uv pip install -U uv
⠧ Resolving dependencies...
^C
```

---

_Comment by @charliermarsh on 2024-05-28 13:13_

You need some way to tell uv which Python to use. If you’re just running the binary directly, we won’t discover the environment interpreter. So yeah, you can either use —python, or activate your environment, or if you name your virtualenv .venv we will discover it automatically.

---

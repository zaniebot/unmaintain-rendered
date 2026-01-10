```yaml
number: 15709
title: "bug: Installation destination inference for `uv pip` commands has inconsistent behavior (`UV_PYTHON` a.k.a `--python`)"
type: issue
state: open
author: gabyx
labels:
  - question
assignees: []
created_at: 2025-09-06T19:21:14Z
updated_at: 2025-09-08T14:18:51Z
url: https://github.com/astral-sh/uv/issues/15709
synced_at: 2026-01-10T03:23:54Z
```

# bug: Installation destination inference for `uv pip` commands has inconsistent behavior (`UV_PYTHON` a.k.a `--python`)

---

_Issue opened by @gabyx on 2025-09-06 19:21_

### Summary

Please reproduce this:


0. Ensure `UV_PYTHON` is no set.

1. Start creating a virtual environment
   `uv venv -p /nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/bin/python3.13` (choose any system python here)

```shell
Using CPython 3.13.6 interpreter at: /nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate.fish
```

Now do one of the following scenrios:

### Scenario A

- Installing numpy `uv pip install numpy -v`
  results in success, and numpy is installed in the **inferred** virtual environment:

  ```shell
  DEBUG uv 0.8.6
  DEBUG Searching for default Python interpreter in virtual environments
  DEBUG Found `cpython-3.13.6-linux-x86_64-gnu` at `/home/gaetan/temp/test_project/.venv/bin/python3` (virtual environment)
  DEBUG Using Python 3.13.6 environment at: .venv
  DEBUG Acquired lock for `.venv`
  DEBUG Requirement satisfied: numpy
  Audited 1 package in 0.73ms
  DEBUG Released lock at `/home/gaetan/temp/test_project/.venv/.lock`
  ```

### Scenario B

- Now explicitly set the Python interpreter (use the same one as in `uv venv`)

  - `export UV_PYTHON=/nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/bin/python3` before and then install
  - `uv pip install numpy -v` fails

  ```shell
  DEBUG uv 0.8.6
  DEBUG Checking for Python interpreter at path `/nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/bin/python3`
  Using Python 3.13.6 environment at: /nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env
  WARN Failed to acquire environment lock: Read-only file system (os error 30) at path "/nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/.tmpx7wOq1"
  DEBUG At least one requirement is not satisfied: numpy
  DEBUG Using request timeout of 30s
  DEBUG Solving with installed Python version: 3.13.6
  DEBUG Solving with target Python version: >=3.13.6
  DEBUG Adding direct dependency: numpy*
  DEBUG Found fresh response for: https://pypi.org/simple/numpy/
  DEBUG Searching for a compatible version of numpy (*)
  DEBUG Selecting: numpy==2.3.2 [compatible] (numpy-2.3.2-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl)
  DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1d/0f/571b2c7a3833ae419fe69ff7b479a78d313581785203cc70a8db90121b9a/numpy-2.3.2-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata
  DEBUG Tried 1 versions: numpy 1
  DEBUG marker environment resolution took 0.005s
  Resolved 1 package in 5ms
  DEBUG Registry requirement already cached: numpy==2.3.2
  DEBUG File already exists (initial attempt), overwriting: /nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/lib/python3.13/site-packages/numpy/linalg/linalg.pyi
  error: Failed to install: numpy-2.3.2-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (numpy==2.3.2)
  Caused by: Read-only file system (os error 30) at path "/nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/lib/python3.13/site-packages/.tmpkD6UTw"
  3m
  ```

> [!NOTE]
> `UV_PYTHON=/nix/store/hwih8a4f4ms88dr3ynh1wcqcpc78pld3-python3-3.13.6-env/bin/python3 uv sync`
> is not impacted by this, and always infers first the virtual environment.

This behavior seems really unintuitive.
It seems that `uv` uses the `UV_PYTHON` env variable also to detect the environment which is slightly confusing.

This is related to the more confusion issue report: #14748 (I closed it)

@GaetanLepage: FYI

### Platform

NixOS

### Version

0.8.6

### Python version

3.13

---

_Label `bug` added by @gabyx on 2025-09-06 19:21_

---

_Renamed from "bug: Installation destination inference for `uv pip` commands has inconsistent behavior" to "bug: Installation destination inference for `uv pip` commands has inconsistent behavior (`UV_PYTHON` a.k.a `--python`)" by @gabyx on 2025-09-06 19:28_

---

_Comment by @charliermarsh on 2025-09-06 19:29_

Hmm, this seems right to me? If you tell us a `--python`, then `uv pip install` attempts to install into that `--python`.

---

_Comment by @gabyx on 2025-09-07 14:46_

Its just weird behavior in the non failing case where you dont set anything then it does discovery, and in the other case it doesnt.

If uv pip truely implements the exact behavior of pip (not sure) then is this wanted? In the success case there is no venv activated! 

---

_Comment by @gabyx on 2025-09-07 14:47_

Is the design of uv going towards "forbid uv pip" -> always enforce an environment , that would be nice.

---

_Comment by @zanieb on 2025-09-07 17:11_

> If uv pip truely implements the exact behavior of pip (not sure) then is this wanted?

I don't think pip has to consider any of these semantics, as it almost always targets the same environment that pip is installed in.

In general, uv won't install into a non-virtual environment without opt-in. However, using `--python` with an explicit path to an interpreter is considered opt-in.

> Its just weird behavior in the non failing case where you dont set anything then it does discovery, and in the other case it doesnt.

I'm not sure what's weird about this. In the first case, we do discovery because you haven't asked for a specific Python interpreter. In the second case, we don't because you've given us a specific path.

I don't think you should set `UV_PYTHON` in this context. It sounds like you want something more like https://github.com/astral-sh/uv/issues/14748? Or you should just use `--python`?


---

_Comment by @gabyx on 2025-09-08 10:00_

Thanks @zanieb : the whole issue comes from #14748 and the fact that `UV_PYTHON` is conceptual the flag `--python` and 
that we as Nix package/devenv maintainer want a way to **really lock** the python **interpreter** which `uv`  uses for **all** commands (no escape hatch).

I think `uv` needs a solution for that cause `UV_PYTHON` has the wrong semantics **for our scenario** at least for `uv pip` and `uv venv` etc.

Whats about `UV_PYTHON_INTERPRETER`?


---

_Comment by @zanieb on 2025-09-08 12:50_

Like you want to assert that the interpreter in a discovered virtual environment matches the value set in a variable?

---

_Comment by @gabyx on 2025-09-08 13:52_

Kind of, and make all uv commands respect that interpreter, when creating a new one with `uv venv` and other command which start fresh or do discovery etc...

---

_Comment by @gabyx on 2025-09-08 13:56_

We can also say we set UV_PYTHON globally to an interpreter not in an environment (/nix/store/...) and forbid the use of `uv pip install` cause it want work. Or UV_PYTHON should not be read on `uv pip` commands. Only `--python`.

---

_Label `bug` removed by @zanieb on 2025-09-08 14:02_

---

_Label `question` added by @zanieb on 2025-09-08 14:02_

---

_Comment by @zanieb on 2025-09-08 14:18_

I think we should be looking more closely at the motivating problem in Nix here. I don't think we should just be hacking more complexity into the `UV_PYTHON` option for this purpose.

---

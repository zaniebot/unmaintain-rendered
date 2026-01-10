```yaml
number: 450
title: Failed to discover site packages directory for PyPy
type: issue
state: closed
author: bendoerry
labels:
  - imports
assignees: []
created_at: 2025-05-19T14:12:13Z
updated_at: 2025-05-20T18:46:51Z
url: https://github.com/astral-sh/ty/issues/450
synced_at: 2026-01-10T02:34:10Z
```

# Failed to discover site packages directory for PyPy

---

_Issue opened by @bendoerry on 2025-05-19 14:12_

### Summary

If I create a venv using PyPy, and then run ty check, I get the following error:

```
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: Could not find the `site-packages` directory for the Python installation at `sys.prefix` path `/some/directory/.venv`
```

If I then copy the directory `.venv/lib/pypy3.11` to `.venv/lib/python3.11`, ty runs as expected.

This venv was created using
```
uv venv --seed --python pypy@3.11
```

### Version

ty 0.0.1-alpha.5

---

_Label `imports` added by @AlexWaygood on 2025-05-19 14:28_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-19 14:38_

---

_Comment by @Mathemmagician on 2025-05-19 14:51_

hi!

---

_Assigned to @Mathemmagician by @AlexWaygood on 2025-05-19 14:51_

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-05-19 14:51_

---

_Comment by @Mathemmagician on 2025-05-19 15:05_

To recap, `.venv` is structured differently depending on how it was created. For example, using uv, or using pypy.

`ty` needs to know where `site-packages` is. Currently, it's looking at a single dir location. pypy uses a different convention, which want to also support. 

Need to double-check if there are any other formats to support.



---

_Comment by @Mathemmagician on 2025-05-19 15:59_

Investigated how virtual environments are created in different ways, summary table of whether `Implementation` flag is present and where `/site-packages` is located:


```
| Virtual Environment      | Has Implementation | Interpreter | UV Installed | Site Packages Path                | Interpreter Path                                          |
|--------------------------|--------------------|-------------|--------------|-----------------------------------|-----------------------------------------------------------|
| .uv-pypy-virtualenv      | Yes                | PyPy        | Yes          | /lib/pypy3.11/site-packages       | ~/.local/share/uv/python/pypy-3.11.11-macos-x86_64-none/bin |
| .virtualenv              | Yes                | CPython     | No           | /lib/python3.10/site-packages     | /Library/Frameworks/Python.framework/Versions/3.10/bin    |
| .python3-venv            | No                 | N/A         | No           | /lib/python3.10/site-packages     | /Library/Frameworks/Python.framework/Versions/3.10/bin    |
| .uv-pypy-venv            | No                 | N/A         | Yes          | /lib/pypy3.11/site-packages       | ~/.local/share/uv/python/pypy-3.11.11-macos-x86_64-none/bin |
| .uv_venv_pypy            | Yes                | PyPy        | Yes          | /lib/pypy3.11/site-packages       | ~/.local/share/uv/python/pypy-3.11.11-macos-x86_64-none/bin |
| .uv                      | Yes                | CPython     | Yes          | /lib/python3.12/site-packages     | ~/.local/share/uv/python/cpython-3.12.10-macos-x86_64-none/bin |
```

The Implementation lookup strategy therefore can be:
1. CPython -> `/lib/pythonX.Y/site-packages`
2. Pypy -> `/lib/pypyX.Y/site-packages`
3. Missing/Other value? -> Check both

Next, will be making a change in [ruff's site_packages.rs](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/site_packages.rs)

Side note - something to double check is that `/site-packages/` might be located at different paths on Windows machines, i.e., `Lib` instead of `lib`


---

_Comment by @AlexWaygood on 2025-05-19 16:00_

> Side note - something to double check is that `/site-packages/` might be located at different paths on Windows machines, i.e., `Lib` instead of `lib`

yup -- we already support this for CPython virtual environments. It should hopefully be similar for PyPy virtual environments!

---

_Comment by @Mathemmagician on 2025-05-19 16:45_

Note for myself to check how poetry fits in here...

---

_Comment by @Skylion007 on 2025-05-19 17:07_

https://github.com/astral-sh/ty/issues/455 Might be a duplicate

---

_Comment by @AlexWaygood on 2025-05-19 21:32_

I checked, and GraalPy virtual environments are also located in the same place as CPython virtual environments, whether they're created with the stdlib `venv` module, the virtualenv library or uv. For GraalPy virtual environments, the `implementation` key is set to `GraalVM` if it is present in the `pyvenv.cfg` file.

---

_Comment by @AlexWaygood on 2025-05-19 21:33_

There are other Python implementations such as RustPython, IronPython and Jython, but for now I think it's okay to limit ourselves to the Python implementations supported by uv, which are CPython, PyPy and GraalPy.

---

_Closed by @AlexWaygood on 2025-05-20 18:46_

---

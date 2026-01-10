```yaml
number: 1905
title: Ignore Python 2 installations when querying for interpreters
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: ignore-python-2
created_at: 2024-02-23T08:46:20Z
updated_at: 2024-02-23T10:55:39Z
url: https://github.com/astral-sh/uv/pull/1905
synced_at: 2026-01-10T14:54:43Z
```

# Ignore Python 2 installations when querying for interpreters

---

_Pull request opened by @MichaReiser on 2024-02-23 08:46_

## Summary

Fixes https://github.com/astral-sh/uv/issues/1693

`uv` currently fails when a user has `python` 2 or older installed on their system without a `python3` or `python3.exe` on their path because
the `get_interpreter_info.py` script fails executing (it uses some Python 3+ APIs).

This PR fixes this by:

* Returning an explicit error code in `get_interpreter_info` if the Python version isn't supported
* Skipping over this error in `python_query` if the user requested ANY python version or a version >= 3. 
* Error if the user requested a Python 2 version. 

## Test Plan

Error if the user requests a legacy python version. 

```
uv venv -p 2
  × Python 2 or older is not supported. Please use Python 3 or newer.
```


Ignore any python 2 installation when querying newer python installations (using v4 here because I have python3 on the path and that takes precedence over querying python)
```
 uv_interpreter::python_query::find_python selector=Major(4)
      0.005541s   0ms DEBUG uv_interpreter::interpreter Detecting markers for: /home/micha/.pyenv/shims/python
      0.059730s  54ms DEBUG uv_interpreter::python_query Found a Python 2 installation that isn't supported by uv, skipping.
      0.059983s  54ms DEBUG uv_interpreter::interpreter Using cached markers for: /usr/bin/python
  × No Python 4 In `PATH`. Is Python 4 installed?

```


---

_Review requested from @konstin by @MichaReiser on 2024-02-23 08:48_

---

_Label `bug` added by @MichaReiser on 2024-02-23 08:49_

---

_Marked ready for review by @MichaReiser on 2024-02-23 08:49_

---

_@konstin approved on 2024-02-23 10:53_

---

_Merged by @konstin on 2024-02-23 10:55_

---

_Closed by @konstin on 2024-02-23 10:55_

---

_Branch deleted on 2024-02-23 10:55_

---

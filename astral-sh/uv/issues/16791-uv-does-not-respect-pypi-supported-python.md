---
number: 16791
title: uv does not respect pypi supported python versions (at least not like pip)
type: issue
state: closed
author: freand76
labels:
  - question
assignees: []
created_at: 2025-11-20T18:38:02Z
updated_at: 2025-11-20T18:57:13Z
url: https://github.com/astral-sh/uv/issues/16791
synced_at: 2026-01-10T01:26:10Z
---

# uv does not respect pypi supported python versions (at least not like pip)

---

_Issue opened by @freand76 on 2025-11-20 18:38_

### Summary

Example, try to install a pypi package with python3.14, The actual package supports  >=3.9,<3.14 according to the pypi database. I am not sure if this is a duplicate bug (I could not find anything similar)  and/or if this is a feature of uv.

**uv output**

$>uv --version
uv 0.9.10

$> uv pip install pyside6==6.10.0
Using Python 3.14.0 environment at: py314
Resolved 4 packages in 120ms
Installed 4 packages in 96ms
 + pyside6==6.10.0
 + pyside6-addons==6.10.0
 + pyside6-essentials==6.10.0
 + shiboken6==6.10.0
$>

**pip output**

$> pip3 --version
pip 25.3 from /.../python3.14/site-packages/pip (python 3.14)

$> pip3 install pyside6==6.10.0
ERROR: Ignored the following versions that require a different python version: 6.0.0 Requires-Python >=3.6,<3.10
 6.0.0a1.dev1606911628 Requires-Python >=3.6,<3.10
 6.0.1 Requires-Python >=3.6,<3.10
 6.0.2 Requires-Python >=3.6,<3.10
 6.0.3 Requires-Python >=3.6,<3.10
 6.0.4 Requires-Python >=3.6,<3.10
 6.1.0 Requires-Python >=3.6,<3.10
 6.1.1 Requires-Python >=3.6,<3.10
 6.1.2 Requires-Python >=3.6,<3.10
 6.1.3 Requires-Python >=3.6,<3.10
 6.10.0 Requires-Python >=3.9,<3.14
 6.2.0 Requires-Python >=3.6,<3.11
 6.2.1 Requires-Python >=3.6,<3.11
 6.2.2 Requires-Python >=3.6,<3.11
 6.2.2.1 Requires-Python >=3.6,<3.11
 6.2.3 Requires-Python >=3.6,<3.11
 6.2.4 Requires-Python >=3.6,<3.11
 6.3.0 Requires-Python >=3.6,<3.11
 6.3.1 Requires-Python >=3.6,<3.11
 6.3.2 Requires-Python >=3.6,<3.11
 6.4.0.1 Requires-Python >=3.7,<3.12
 6.4.1 Requires-Python >=3.7,<3.12
 6.4.2 Requires-Python >=3.7,<3.12
 6.4.3 Requires-Python >=3.7,<3.12
 6.5.0 Requires-Python >=3.7,<3.12
 6.5.1 Requires-Python >=3.7,<3.12
 6.5.1.1 Requires-Python >=3.7,<3.12
 6.5.2 Requires-Python >=3.7,<3.12
 6.5.3 Requires-Python >=3.7,<3.12
 6.6.0 Requires-Python >=3.8,<3.13
 6.6.1 Requires-Python >=3.8,<3.13
 6.6.2 Requires-Python >=3.8,<3.13
 6.6.3 Requires-Python >=3.8,<3.13
 6.6.3.1 Requires-Python >=3.8,<3.13
 6.7.0 Requires-Python >=3.9,<3.13
 6.7.1 Requires-Python >=3.9,<3.13
 6.7.2 Requires-Python >=3.9,<3.13
 6.7.3 Requires-Python >=3.9,<3.13
 6.8.0 Requires-Python >=3.9,<3.13
 6.8.0.1 Requires-Python >=3.9,<3.13
 6.8.0.2 Requires-Python >=3.9,<3.14
 6.8.1 Requires-Python >=3.9,<3.14
 6.8.1.1 Requires-Python >=3.9,<3.14
 6.8.2 Requires-Python >=3.9,<3.14
 6.8.2.1 Requires-Python >=3.9,<3.14
 6.8.3 Requires-Python >=3.9,<3.14
 6.9.0 Requires-Python >=3.9,<3.14
 6.9.1 Requires-Python >=3.9,<3.14
 6.9.2 Requires-Python >=3.9,<3.14
 6.9.3 Requires-Python >=3.9,<3.14
ERROR: Could not find a version that satisfies the requirement pyside6==6.10.0 (from versions: 6.10.1)
ERROR: No matching distribution found for pyside6==6.10.0

$>


### Platform

ubuntu 24.04

### Version

0.9.10

### Python version

python 3.14.0

---

_Label `bug` added by @freand76 on 2025-11-20 18:38_

---

_Comment by @zanieb on 2025-11-20 18:41_

Please see https://docs.astral.sh/uv/pip/compatibility/#requires-python-upper-bounds

---

_Label `bug` removed by @zanieb on 2025-11-20 18:41_

---

_Label `question` added by @zanieb on 2025-11-20 18:41_

---

_Comment by @freand76 on 2025-11-20 18:56_

Thank you for quick response.

---

_Closed by @freand76 on 2025-11-20 18:57_

---

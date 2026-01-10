```yaml
number: 6004
title: Update URL to distutils configuration files docs
type: pull_request
state: merged
author: edmorley
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: distuils-docs-url
created_at: 2024-08-11T10:57:38Z
updated_at: 2024-08-19T10:54:06Z
url: https://github.com/astral-sh/uv/pull/6004
synced_at: 2026-01-10T13:09:50Z
```

# Update URL to distutils configuration files docs

---

_Pull request opened by @edmorley on 2024-08-11 10:57_

## Summary

The existing URL 404s:
https://docs.python.org/3/install/index.html#distutils-configuration-files

...since the `/3/` route now resolves to Python 3.12, where `distutils` has been removed:
https://docs.python.org/3.12/whatsnew/3.12.html#distutils

The Python 3.11 docs are the most recent where the page still exists:
https://docs.python.org/3.11/install/index.html#distutils-configuration-files

## Test Plan

N/A


---

_Comment by @edmorley on 2024-08-11 11:00_

(I presume even though newer Python doesn't include `distutils`, the `uv-virtualenv` crate's distutils-patching behaviour inherited from [gourgeist](https://github.com/konstin/gourgeist) is still needed, since setuptools now bundles `distutils`?)

---

_@zanieb approved on 2024-08-11 15:04_

---

_Label `documentation` added by @zanieb on 2024-08-11 15:04_

---

_Label `internal` added by @zanieb on 2024-08-11 15:04_

---

_Comment by @zanieb on 2024-08-11 15:04_

Not certain regarding your question, cc @konstin 

---

_Comment by @konstin on 2024-08-19 09:47_

> I presume even though newer Python doesn't include distutils, the uv-virtualenv crate's distutils-patching behaviour inherited from [gourgeist](https://github.com/konstin/gourgeist) is still needed, since setuptools now bundles distutils?

The file is vendored from https://github.com/pypa/virtualenv/blob/main/src/virtualenv/create/via_global_ref/_virtualenv.py, iirc we still need it for setuptools.

---

_@konstin approved on 2024-08-19 09:47_

---

_Merged by @konstin on 2024-08-19 09:48_

---

_Closed by @konstin on 2024-08-19 09:48_

---

_Branch deleted on 2024-08-19 10:54_

---

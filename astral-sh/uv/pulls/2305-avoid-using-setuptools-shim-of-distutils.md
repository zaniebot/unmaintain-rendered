```yaml
number: 2305
title: Avoid using setuptools shim of distutils
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/patch
created_at: 2024-03-08T19:41:22Z
updated_at: 2024-03-08T19:53:50Z
url: https://github.com/astral-sh/uv/pull/2305
synced_at: 2026-01-10T14:54:43Z
```

# Avoid using setuptools shim of distutils

---

_Pull request opened by @charliermarsh on 2024-03-08 19:41_

## Summary

It turns out that setuptools includes a shim to patch distutils. I'll admit that I don't fully understand why or how it's different, but this is the trick `pip` uses to ensure that it gets the "original" distutils.

We actually use distutils in two places: once for the system Python scheme, and once for virtual environments. In virtualenv, they _do_ use the patched distutils, so this could deviate in ways I don't understand.

Closes #2302.


---

_Label `compatibility` added by @charliermarsh on 2024-03-08 19:41_

---

_Label `bug` added by @charliermarsh on 2024-03-08 19:41_

---

_Comment by @charliermarsh on 2024-03-08 19:42_

@gaborbernat - do you know if it's necessary for virtualenv to use the patched version of `distutils`? I see a note in https://github.com/pypa/virtualenv/pull/1771, but I can't tell if it's _required_ or merely necessary for others that might import virtualenv.

---

_Comment by @potiuk on 2024-03-08 19:45_

Nice!

---

_Comment by @gaborbernat on 2024-03-08 19:45_

I believe this aligns with how packages behave. pip uses setuptools distutils historically for installing packages. So without that shim you might end up with bug reports of installations ending up in expected locations when the OS patched distutils and setuptools distutils  information differs (primarily in debian).

---

_Merged by @charliermarsh on 2024-03-08 19:53_

---

_Closed by @charliermarsh on 2024-03-08 19:53_

---

_Branch deleted on 2024-03-08 19:53_

---

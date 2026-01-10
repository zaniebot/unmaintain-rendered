---
number: 12524
title: "Why uv gives \"No interpreter found in registry or search path\" when running `uv venv --python-preference only-system`?"
type: issue
state: closed
author: buptsad
labels:
  - question
assignees: []
created_at: 2025-03-28T04:31:18Z
updated_at: 2025-04-01T19:39:39Z
url: https://github.com/astral-sh/uv/issues/12524
synced_at: 2026-01-10T01:25:20Z
---

# Why uv gives "No interpreter found in registry or search path" when running `uv venv --python-preference only-system`?

---

_Issue opened by @buptsad on 2025-03-28 04:31_

### Question

I installed uv binary file a week ago from github release, and when I tried to create a environment with `uv venv --python-preference only-system`, it always throws an error "× No interpreter found in registry or search path". How to solve this?

### Platform

Win11

### Version

uv 0.6.9 (3d9460278 2025-03-20)

---

_Label `question` added by @buptsad on 2025-03-28 04:31_

---

_Comment by @FishAlchemist on 2025-03-28 06:57_

python-preference for ``only-system``
``only-system``: Only use system Python installations; never use managed Python installations.
Ref: https://docs.astral.sh/uv/reference/settings/#python-preference

Discovery of system Python installations 
* A Python interpreter on the PATH as python, ``python.exe`` on Windows.
* On Windows, the Python interpreters in the Windows registry and Microsoft Store Python interpreters (see py --list-paths) that match the requested version.

Ref: https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions

In simple terms, if you choose ``only-system``, you must ensure that Python can be found by the sources mentioned above.

You can use ``uv python list --only-installed`` to find out which installed Python versions uv has detected.
You can also use ``uv python list --no-managed-python`` to find out which installed system Python versions uv has detected.

---

_Closed by @zanieb on 2025-04-01 19:39_

---

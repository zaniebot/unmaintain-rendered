```yaml
number: 13684
title: Add note on uv venv replacing the existing virtual environment
type: pull_request
state: open
author: stevenae
labels: []
assignees: []
base: main
head: patch-3
created_at: 2025-05-27T15:16:57Z
updated_at: 2025-06-16T14:56:00Z
url: https://github.com/astral-sh/uv/pull/13684
synced_at: 2026-01-12T16:10:48Z
```

# Add note on uv venv replacing the existing virtual environment

---

_@stevenae_

## Summary

Uv venv replaces existing .venv. There has been confusion about this, see  #1472. Determined this would be the right place to make an edit, per #13624.

## Test Plan

Followed instructions at https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md#documentation


---

_@zanieb reviewed on 2025-05-27 15:46_

---

_Review comment by @zanieb on `docs/pip/environments.md`:20 on 2025-05-27 15:46_

A couple problems here:

1. This is true for `uv venv <name>` too.
2. This is not quite correct. uv will only overwrite existing virtual environments, e.g.:

```
❯ mkdir foobar
❯ touch foobar/foo
❯ uv venv foobar
Using CPython 3.13.3
Creating virtual environment at: foobar
uv::venv::creation

  × Failed to create virtualenv
  ╰─▶ The directory `foobar` exists, but it's not a virtual environment
```

---

_Assigned to @zanieb by @zanieb on 2025-05-28 14:27_

---

_@stevenae reviewed on 2025-06-03 15:21_

---

_Review comment by @stevenae on `docs/pip/environments.md`:20 on 2025-06-03 15:21_

Tried to address these comments!

---

_Comment by @stevenae on 2025-06-10 13:00_

@zanieb are you able to review?

---

_@stevenae reviewed on 2025-06-16 14:56_

---

_Review comment by @stevenae on `docs/pip/environments.md`:20 on 2025-06-16 14:56_

@zanieb are you able to look? Would love to land if it's viable!

---

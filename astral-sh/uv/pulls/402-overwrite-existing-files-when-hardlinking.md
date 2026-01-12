```yaml
number: 402
title: Overwrite existing files when hardlinking
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/hardlink
created_at: 2023-11-10T20:19:57Z
updated_at: 2023-11-10T20:24:21Z
url: https://github.com/astral-sh/uv/pull/402
synced_at: 2026-01-12T16:03:55Z
```

# Overwrite existing files when hardlinking

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/puffin/issues/390.

## Test Plan

Installed `jupyter_core==5.5.0`, then removed the `jupyter_core` and `jupyter_core-5.5.0.dist-info` directories from my virtualenv manually, but left `jupyter.py`. I then re-ran `puffin pip-compile`, and verified that it errored on `main` but succeeded here.

---

_Label `bug` added by @charliermarsh on 2023-11-10 20:20_

---

_Merged by @charliermarsh on 2023-11-10 20:24_

---

_Closed by @charliermarsh on 2023-11-10 20:24_

---

_Branch deleted on 2023-11-10 20:24_

---

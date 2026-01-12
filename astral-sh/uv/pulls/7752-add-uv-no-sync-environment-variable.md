```yaml
number: 7752
title: "Add `UV_NO_SYNC` environment variable"
type: pull_request
state: merged
author: adisbladis
labels:
  - configuration
assignees: []
merged: true
base: main
head: uv_no_sync_env
created_at: 2024-09-28T04:36:14Z
updated_at: 2024-09-28T16:03:48Z
url: https://github.com/astral-sh/uv/pull/7752
synced_at: 2026-01-12T16:07:58Z
```

# Add `UV_NO_SYNC` environment variable

---

_@adisbladis_

## Summary

I have a workflow where I want use `uv` as a dependency solver only, and manage my environments with external tooling (Nix).

## Test Plan

Manually tested. Automated testing seems excessive for such a trivial change.

## Problems

It's still not as useful as I'd like it to be.
`uv` uncondtionally creates a virtual environment, something I would expect that `--no-sync` should disable.
This looks a bit more tricky to achieve and I'm not sure about how to best structure it.

---

_Comment by @charliermarsh on 2024-09-28 12:36_

I think `--no-sync` should _probably_ avoid creating a virtual environment.

---

_@charliermarsh approved on 2024-09-28 16:03_

---

_Merged by @charliermarsh on 2024-09-28 16:03_

---

_Closed by @charliermarsh on 2024-09-28 16:03_

---

_Label `configuration` added by @charliermarsh on 2024-09-28 16:03_

---

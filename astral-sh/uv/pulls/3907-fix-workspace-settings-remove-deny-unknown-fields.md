```yaml
number: 3907
title: Fix workspace settings - remove deny unknown fields
type: pull_request
state: merged
author: blueraft
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: fix-toml-settings
created_at: 2024-05-29T15:23:57Z
updated_at: 2024-05-29T15:31:07Z
url: https://github.com/astral-sh/uv/pull/3907
synced_at: 2026-01-10T13:59:34Z
```

# Fix workspace settings - remove deny unknown fields

---

_Pull request opened by @blueraft on 2024-05-29 15:23_

## Summary

This PR addresses an issue where `tool.uv` settings are not read if `tool.uv.sources` or `tool.uv.workspaces` are present in the TOML file. 

## Test Plan

Tested locally.

---

_@charliermarsh approved on 2024-05-29 15:31_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-05-29 15:31_

---

_Label `configuration` added by @charliermarsh on 2024-05-29 15:31_

---

_Merged by @charliermarsh on 2024-05-29 15:31_

---

_Closed by @charliermarsh on 2024-05-29 15:31_

---

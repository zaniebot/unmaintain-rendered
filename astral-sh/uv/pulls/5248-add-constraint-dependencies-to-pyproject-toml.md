```yaml
number: 5248
title: Add constraint dependencies to pyproject.toml
type: pull_request
state: merged
author: Di-Is
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: support-constraint-in-pyproject
created_at: 2024-07-20T14:46:01Z
updated_at: 2024-08-24T09:07:19Z
url: https://github.com/astral-sh/uv/pull/5248
synced_at: 2026-01-12T16:06:42Z
```

# Add constraint dependencies to pyproject.toml

---

_@Di-Is_

Resolves #4467.

## Summary

This PR implements the following

1. Add `tool.uv.constraint-dependencies` to pyproject.toml
1. Support to refer `tool.uv.constraint-dependencies` in `uv lock`
1. Support to refer `tool.uv.constraint-dependencies` in `uv pip compile/install`

These are analogues of the override features implemented in #3839 and #4369.

## Test Plan

Add test.

---

_Renamed from "Use constraint dependency from workspace setting file with the `uv lock`" to "Support constraint dependencies in `uv lock`." by @Di-Is on 2024-07-20 14:46_

---

_Renamed from "Support constraint dependencies in `uv lock`." to "Support constraint dependencies in `uv lock`" by @Di-Is on 2024-07-20 14:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-20 15:43_

---

_Comment by @charliermarsh on 2024-07-20 16:29_

Thanks, this is looking great! Do you mind adding the analogous changes to `crates/uv/src/settings.rs`? See `overrides_from_workspace`.

---

_Renamed from "Support constraint dependencies in `uv lock`" to "Add constraint dependencies to pyproject.toml" by @Di-Is on 2024-07-21 02:46_

---

_Comment by @Di-Is on 2024-07-21 23:43_

I modified `crates/uv/src/settings.rs` to reference `tool.uv.constraint-dependencies`.

---

_@charliermarsh approved on 2024-07-21 23:45_

Thank you.

---

_Merged by @charliermarsh on 2024-07-21 23:45_

---

_Closed by @charliermarsh on 2024-07-21 23:45_

---

_Label `enhancement` added by @charliermarsh on 2024-07-21 23:45_

---

_Label `preview` added by @charliermarsh on 2024-07-21 23:45_

---

_Branch deleted on 2024-07-22 11:00_

---

_Comment by @purajit on 2024-08-24 09:07_

Incredible! We had one use-case where we might have been forced to use the pip interface, I didn't see anything in the docs, but luckily Google surfaced this PR! Works perfectly.

---

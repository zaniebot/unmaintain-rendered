---
number: 6810
title: "Extend `uv tree` with options to support `johnnydep` use case"
type: issue
state: open
author: nathanscain
labels:
  - enhancement
assignees: []
created_at: 2024-08-29T12:47:38Z
updated_at: 2024-08-29T13:52:43Z
url: https://github.com/astral-sh/uv/issues/6810
synced_at: 2026-01-07T13:12:17-06:00
---

# Extend `uv tree` with options to support `johnnydep` use case

---

_Issue opened by @nathanscain on 2024-08-29 12:47_

I'm very flexible on the interface and currently using a workaround of creating throwaway environments to list out dependencies, but it would be nice if I could just run something like `uv tree fastapi[standard]` and get a cross-platform dependency list out for that package.

This is a common use case for managing offline environments where software generally needs to be submitted for approval before it can be added onto a system.

The `uv` resolver should make this faster, and this workflow would allow the tree to be generated without needing to download all the files - just the metadata.

---

_Label `enhancement` added by @charliermarsh on 2024-08-29 13:52_

---

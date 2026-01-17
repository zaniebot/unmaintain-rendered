```yaml
number: 17546
title: Refactor arguments that parses requirements to use HashSet instead of Vec
type: issue
state: open
author: gaardhus
labels:
  - enhancement
assignees: []
created_at: 2026-01-17T09:09:44Z
updated_at: 2026-01-17T09:13:02Z
url: https://github.com/astral-sh/uv/issues/17546
synced_at: 2026-01-17T10:08:04Z
```

# Refactor arguments that parses requirements to use HashSet instead of Vec

---

_@gaardhus_

### Summary

## Description

Currently, several commands use `Vec<PackageName>` for their `exclude` arguments, including:
- `PipListSettings` 
- Other similar settings structures

This should be refactored to use `HashSet<PackageName>` instead for better performance when checking exclusions.

## Context

This was identified during review of #17045, which implements the `--exclude` flag for `uv pip freeze`. The implementation correctly uses `HashSet` for the exclude parameter, but other similar cases in the codebase still use `Vec`.

## Proposed Solution

- Audit the codebase for `exclude` parameters that use `Vec<PackageName>`
- Convert them to `HashSet<PackageName>` for faster lookups
- Update any related code that constructs or uses these exclude lists

## References

- PR #17045 (comment thread: https://github.com/astral-sh/uv/pull/17045/files#r2614916235)
- Specifically mentioned: `PipListSettings` has a similar exclude argument that is currently a `Vec`

### Example

_No response_

---

_Label `enhancement` added by @gaardhus on 2026-01-17 09:09_

---

_Renamed from "Refactor `exclude` arguments to use HashSet instead of Vec" to "Refactor arguments that parses requirements to use HashSet instead of Vec" by @gaardhus on 2026-01-17 09:13_

---

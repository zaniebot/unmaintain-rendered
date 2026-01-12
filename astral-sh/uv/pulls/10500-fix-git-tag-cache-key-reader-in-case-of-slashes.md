```yaml
number: 10500
title: "Fix git-tag cache-key reader in case of slashes (#10467)"
type: pull_request
state: merged
author: snizovtsev
labels:
  - bug
assignees: []
merged: true
base: main
head: snizovtsev/fix-slashed-git-tags
created_at: 2025-01-11T15:47:21Z
updated_at: 2025-01-12T02:30:46Z
url: https://github.com/astral-sh/uv/pull/10500
synced_at: 2026-01-12T16:09:19Z
```

# Fix git-tag cache-key reader in case of slashes (#10467)

---

_@snizovtsev_

## Summary

The assumption that all tags are listed under a flat `.git/ref/tags` structure was wrong. Git creates a hierarchy of directories for tags containing slashes. To fix the cache key calculation, we need to recursively traverse all files under that folder instead.

## Test Plan

1. Create an `uv` project with git-tag cache-keys;
2. Add any tag with slash;
3. Run `uv sync` and see uv_cache_info error in verbose log;
4. `uv sync` doesn't trigger reinstall on next tag addition or removal;
5. With fix applied, reinstall triggers on every tag update and there are no errors in the log.

Fixes #10467

---

_Converted to draft by @snizovtsev on 2025-01-11 16:06_

---

_Marked ready for review by @snizovtsev on 2025-01-11 16:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-12 01:37_

---

_@charliermarsh approved on 2025-01-12 01:38_

Thanks, that's interesting -- TIL.

---

_Label `bug` added by @charliermarsh on 2025-01-12 02:02_

---

_Merged by @charliermarsh on 2025-01-12 02:30_

---

_Closed by @charliermarsh on 2025-01-12 02:30_

---

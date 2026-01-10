```yaml
number: 1325
title: Fix dataclass_transform parameter overrides for kw_only and frozen
type: pull_request
state: closed
author: fatelei
labels: []
assignees: []
base: main
head: issue-1260
created_at: 2025-10-08T10:11:05Z
updated_at: 2025-10-08T10:16:36Z
url: https://github.com/astral-sh/ty/pull/1325
synced_at: 2026-01-10T02:34:10Z
```

# Fix dataclass_transform parameter overrides for kw_only and frozen

---

_Pull request opened by @fatelei on 2025-10-08 10:11_

- Add support for eq, kw_only, and frozen parameter overrides in @dataclass_transform
- Previously only order parameter override was supported
- Update test documentation to reflect fixed behavior
- Resolves issue where kw_only_default and frozen_default could not be overridden

The fix is in the ruff submodule at commit 632e63b3d1

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The fix resolves the issue where@tp.dataclass_transform(kw_only_default=True,frozen_default=True) wasn't properly handling parameter overrides

## Test Plan

<!-- How was it tested? -->


---

_Closed by @fatelei on 2025-10-08 10:16_

---

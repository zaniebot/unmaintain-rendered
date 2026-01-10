```yaml
number: 20140
title: "[ty] Add more specific diagnostics for unsupported comparison diagnostics"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
  - diagnostics
assignees: []
base: main
head: unsupported-operator-diagnostics
created_at: 2025-08-28T18:46:27Z
updated_at: 2026-01-09T02:09:00Z
url: https://github.com/astral-sh/ruff/pull/20140
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Add more specific diagnostics for unsupported comparison diagnostics

---

_Pull request opened by @MatthewMckee4 on 2025-08-28 18:46_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/921

We now emit an extra info diagnostic explaining why the rich comparison is not supported.

## Test Plan

Add tests to `crates/ty_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`


---

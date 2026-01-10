```yaml
number: 15020
title: "fix: remove mdformat-admon and custom no-escape plugin"
type: pull_request
state: merged
author: KyleKing
labels:
  - documentation
assignees: []
merged: true
base: renovate/mdformat-mkdocs-4.x
head: renovate-patch/mdformat-mkdocs-v4
created_at: 2024-12-16T13:12:43Z
updated_at: 2024-12-16T17:02:27Z
url: https://github.com/astral-sh/ruff/pull/15020
synced_at: 2026-01-10T20:42:27Z
```

# fix: remove mdformat-admon and custom no-escape plugin

---

_Pull request opened by @KyleKing on 2024-12-16 13:12_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolve mdformat issues raised while upgrading to `mdformat-mkdocs` in #15011

## Test Plan

```sh
> uv run --no-project --isolated --with-requirements docs/requirements.txt scripts/generate_mkdocs.py
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.33s
     Running `target/debug/ruff_dev generate-docs`
Rule F401 references deprecated option lint.ignore-init-module-imports.
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff_dev generate-rules-table`
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff_dev generate-options`
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff rule --all --output-format json`  
```

---

_Comment by @github-actions[bot] on 2024-12-16 13:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-12-16 13:22_

---

_Label `documentation` added by @dhruvmanila on 2024-12-16 17:00_

---

_@dhruvmanila approved on 2024-12-16 17:02_

Thank you!

---

_Merged by @dhruvmanila on 2024-12-16 17:02_

---

_Closed by @dhruvmanila on 2024-12-16 17:02_

---

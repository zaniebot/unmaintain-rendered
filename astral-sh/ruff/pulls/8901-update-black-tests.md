```yaml
number: 8901
title: Update Black tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: update-black-tests
created_at: 2023-11-29T05:51:41Z
updated_at: 2023-11-30T00:09:56Z
url: https://github.com/astral-sh/ruff/pull/8901
synced_at: 2026-01-10T23:40:55Z
```

# Update Black tests

---

_Pull request opened by @MichaReiser on 2023-11-29 05:51_

## Summary

Updates all black tests. Black centralized their tests into the `cases` directory which is why we see many moved test files.

There's one unstable test. I commented it out for now and created [an issue](https://github.com/astral-sh/ruff/issues/8905) to investigate the formatting. 

## Test Plan

cargo test


---

_Comment by @MichaReiser on 2023-11-29 05:51_

Current dependencies on/for this PR:
* main
  * **PR #8901** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8901?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8901?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-11-29 06:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `internal` added by @MichaReiser on 2023-11-29 06:53_

---

_Marked ready for review by @MichaReiser on 2023-11-29 07:03_

---

_@charliermarsh approved on 2023-11-29 14:50_

---

_@konstin approved on 2023-11-29 15:46_

Thanks for updating this

---

_Merged by @MichaReiser on 2023-11-30 00:09_

---

_Closed by @MichaReiser on 2023-11-30 00:09_

---

_Branch deleted on 2023-11-30 00:09_

---

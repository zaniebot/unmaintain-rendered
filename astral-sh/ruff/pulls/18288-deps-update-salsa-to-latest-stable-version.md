```yaml
number: 18288
title: "deps: Update salsa to latest stable version"
type: pull_request
state: closed
author: ognevny
labels: []
assignees: []
base: main
head: update-salsa
created_at: 2025-05-24T16:12:07Z
updated_at: 2025-05-25T06:42:08Z
url: https://github.com/astral-sh/ruff/pull/18288
synced_at: 2026-01-10T18:51:02Z
```

# deps: Update salsa to latest stable version

---

_Pull request opened by @ognevny on 2025-05-24 16:12_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

it includes required change, and crate from crates.io should be fetched without [issues](https://github.com/astral-sh/ruff/pull/18212#issuecomment-2906592171). somwhow it fetched on my Linux machine
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-24 16:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-24 16:19_

Thank you. But we prefer to keep the git dependency for now as we're still actively iterating on salsa

---

_Closed by @MichaReiser on 2025-05-24 16:19_

---

_Comment by @ognevny on 2025-05-24 16:20_

> Thank you. But we prefer to keep the git dependency for now as we're still actively iterating on salsa

ok, I'll use my patch downstream and remove it when the issue dissappears

---

_Branch deleted on 2025-05-24 16:20_

---

_Comment by @github-actions[bot] on 2025-05-24 16:21_

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

_Comment by @ognevny on 2025-05-25 06:42_

turns out it was very strange issue with github and cargo's git fetching. changing config to use git CLI helps

---

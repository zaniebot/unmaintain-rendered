```yaml
number: 13743
title: Downgrade benchmarks CI job to ubuntu 22
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/ci-fix-benchmarks-ci
created_at: 2024-10-14T07:06:21Z
updated_at: 2024-10-14T07:28:40Z
url: https://github.com/astral-sh/ruff/pull/13743
synced_at: 2026-01-10T20:59:37Z
```

# Downgrade benchmarks CI job to ubuntu 22

---

_Pull request opened by @MichaReiser on 2024-10-14 07:06_

## Summary

GitHub is rolling out their ubuntu 24 runner image but Codspeed doesn't support ubuntu 24 yet. 

"Downgrade" the benchmarks runner to ubuntu 22.

I opened a [feature request](https://discord.com/channels/1065233827569598464/1295280911205666817/1295280911205666817) in codspeed's discord for ubuntu 24 support. 

## Test Plan

See GitHub checks


---

_Label `ci` added by @MichaReiser on 2024-10-14 07:06_

---

_Merged by @MichaReiser on 2024-10-14 07:17_

---

_Closed by @MichaReiser on 2024-10-14 07:17_

---

_Branch deleted on 2024-10-14 07:17_

---

_Comment by @github-actions[bot] on 2024-10-14 07:28_

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

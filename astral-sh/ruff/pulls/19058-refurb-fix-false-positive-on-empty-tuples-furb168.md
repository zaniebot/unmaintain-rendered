```yaml
number: 19058
title: "[`refurb`] Fix false positive on empty tuples (`FURB168`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-FURB168-empty-tuple-false-positive
created_at: 2025-06-30T23:48:30Z
updated_at: 2025-07-02T17:11:41Z
url: https://github.com/astral-sh/ruff/pull/19058
synced_at: 2026-01-10T18:33:12Z
```

# [`refurb`] Fix false positive on empty tuples (`FURB168`)

---

_Pull request opened by @MeGaGiGaGon on 2025-06-30 23:48_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR fixes #19047 / the [isinstance-type-none (FURB168)](https://docs.astral.sh/ruff/rules/isinstance-type-none/#isinstance-type-none-furb168) tuple false positive by adding a check if the tuple is empty to the code. I also noticed there was another false positive with the other tuple check in the same function, so I fixed it the same way. `Union[()]` is invalid at runtime with `TypeError: Cannot take a Union of no types.`, but it is accepted by `basedpyright` [playground](https://basedpyright.com/?pythonVersion=3.8&typeCheckingMode=all&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCqKSYKAsAFAgCmAbtQIYA2A%2BvAtQBREkoDanAJQBdQUA) and is equivalent to `Never`, so I fixed it anyways. I'm getting on a side tangent here, but it looks like MyPy doesn't accept it, and ty [playground](https://play.ty.dev/c2c468b6-38e4-4dd9-a9fa-0276e843e395) gives `@Todo`.

## Test Plan

<!-- How was it tested? -->

Added two test cases for the two false positives. [playground](https://play.ruff.rs/a53afc21-9a1d-4b9b-9346-abfbeabeb449)

---

_Renamed from "Fix furb168 empty tuple false positive" to "[`refurb`] Fix `FURB168` empty tuple false positive" by @MeGaGiGaGon on 2025-06-30 23:49_

---

_Comment by @github-actions[bot] on 2025-06-30 23:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-07-01 14:21_

---

_Label `rule` added by @ntBre on 2025-07-01 14:21_

---

_@ntBre approved on 2025-07-01 14:25_

Thanks!

---

_Renamed from "[`refurb`] Fix `FURB168` empty tuple false positive" to "[`refurb`] Fix false positive on empty tuples (`FURB168`)" by @ntBre on 2025-07-01 14:26_

---

_Merged by @ntBre on 2025-07-01 14:26_

---

_Closed by @ntBre on 2025-07-01 14:26_

---

_Branch deleted on 2025-07-02 17:11_

---

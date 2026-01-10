```yaml
number: 21478
title: "[`ruff`] Avoid false positive on `ClassVar` reassignment (`RUF012`)"
type: pull_request
state: merged
author: Ruchir28
labels:
  - rule
assignees: []
merged: true
base: main
head: main
created_at: 2025-11-16T15:28:33Z
updated_at: 2025-11-17T20:52:25Z
url: https://github.com/astral-sh/ruff/pull/21478
synced_at: 2026-01-10T16:53:56Z
```

# [`ruff`] Avoid false positive on `ClassVar` reassignment (`RUF012`)

---

_Pull request opened by @Ruchir28 on 2025-11-16 15:28_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #21389

Avoid RUF012 false positives when reassigning a ClassVar

## Test Plan

<!-- How was it tested? -->

Added the new reassignment scenario to `crates/ruff_linter/resources/test/fixtures/ruff/RUF012.py`.

---

_Renamed from "[lint] Enhance RUF012 rule to handle ClassVar reassignment" to "`RUF012` handle ClassVar reassignment" by @MichaReiser on 2025-11-17 08:04_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@ntBre approved on 2025-11-17 20:39_

Thank you! 

I was initially picturing an approach where we looked up the assignment target in the enclosing scope to see if it had previously been the target of an annotated assignment, but I don't think that works because this lint rule runs on deferred scopes. So the approach here makes sense to me instead.

I pushed a commit fixing a couple of small nits:
- Avoid converting the `name.id` to a String
- Combine the new if-let with the existing `AnnAssign` match arm
- Combine the new `class_var_targets.contains` check with the old `iter().all()` pattern

---

_Label `rule` added by @ntBre on 2025-11-17 20:40_

---

_Renamed from "`RUF012` handle ClassVar reassignment" to "[`ruff`] Avoid false positive on `ClassVar` reassignment (`RUF012`)" by @ntBre on 2025-11-17 20:52_

---

_Merged by @ntBre on 2025-11-17 20:52_

---

_Closed by @ntBre on 2025-11-17 20:52_

---

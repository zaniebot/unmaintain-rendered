```yaml
number: 12957
title: "[`pylint`] - remove AugAssign errors from `self-cls-assignment` (`W0642`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: remove-augassign-from-plw0642
created_at: 2024-08-17T20:08:10Z
updated_at: 2024-08-18T15:40:11Z
url: https://github.com/astral-sh/ruff/pull/12957
synced_at: 2026-01-12T15:55:42Z
```

# [`pylint`] - remove AugAssign errors from `self-cls-assignment` (`W0642`)

---

_@diceroll123_

## Summary

Fixes #12954 

## Test Plan

`cargo test`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pylint/self_or_cls_assignment.py`:6 on 2024-08-17 20:10_

Maybe we should leave the fixture exactly as it is (but change the `# PLW0642` comments to `# OK, augmented assignments are ignored`), so that the snapshot "asserts" that augmented assignments _don't_ trigger the rule

---

_@AlexWaygood reviewed on 2024-08-17 20:10_

Thanks, looks good!

---

_@diceroll123 reviewed on 2024-08-17 20:13_

---

_Review comment by @diceroll123 on `crates/ruff_linter/resources/test/fixtures/pylint/self_or_cls_assignment.py`:6 on 2024-08-17 20:13_

Silly me!

---

_Comment by @codspeed-hq[bot] on 2024-08-17 20:14_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:remove-augassign-from-plw0642)

### Merging #12957 will **not alter performance**

<sub>Comparing <code>diceroll123:remove-augassign-from-plw0642</code> (f982598) with <code>main</code> (900e98b)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-17 20:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-18 15:27_

Thanks!

---

_Label `bug` added by @AlexWaygood on 2024-08-18 15:27_

---

_Label `rule` added by @AlexWaygood on 2024-08-18 15:27_

---

_Merged by @AlexWaygood on 2024-08-18 15:31_

---

_Closed by @AlexWaygood on 2024-08-18 15:31_

---

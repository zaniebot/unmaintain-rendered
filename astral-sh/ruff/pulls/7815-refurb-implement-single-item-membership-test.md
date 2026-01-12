```yaml
number: 7815
title: "[`refurb`] Implement `single-item-membership-test` (`FURB171`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: single-in
created_at: 2023-10-04T15:52:23Z
updated_at: 2023-10-08T19:41:50Z
url: https://github.com/astral-sh/ruff/pull/7815
synced_at: 2026-01-12T15:55:24Z
```

# [`refurb`] Implement `single-item-membership-test` (`FURB171`)

---

_@tjkuson_

## Summary

Implement [`no-single-item-in`](https://github.com/dosisod/refurb/blob/master/refurb/checks/iterable/no_single_item_in.py) as `single-item-membership-test` (`FURB171`).

Uses the helper function `generate_comparison` from the `pycodestyle` implementations; this function should probably be moved, but I am not sure where at the moment.

Update: moved it to `ruff_python_ast::helpers`.

Related to #1348.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-10-04 16:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @tjkuson on 2023-10-04 16:24_

---

_Comment by @codspeed-hq[bot] on 2023-10-04 16:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:single-in)

### Merging #7815 will **not alter performance**

<sub>Comparing <code>tjkuson:single-in</code> (a067e8e) with <code>main</code> (bdd925c)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Label `rule` added by @charliermarsh on 2023-10-08 13:59_

---

_Comment by @charliermarsh on 2023-10-08 13:59_

Thanks, looks great.

---

_Merged by @charliermarsh on 2023-10-08 14:08_

---

_Closed by @charliermarsh on 2023-10-08 14:08_

---

_Branch deleted on 2023-10-08 19:41_

---

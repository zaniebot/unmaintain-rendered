```yaml
number: 7617
title: "[`refurb`] Implement `print-empty-string` (`FURB105`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: print-empty-string
created_at: 2023-09-23T13:27:30Z
updated_at: 2023-09-28T19:59:12Z
url: https://github.com/astral-sh/ruff/pull/7617
synced_at: 2026-01-12T02:39:10Z
```

# [`refurb`] Implement `print-empty-string` (`FURB105`)

---

_Pull request opened by @tjkuson on 2023-09-23 13:27_

## Summary

Implement [`simplify-print`](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/print.py) as `print-empty-string` (`FURB105`).

Extends the original rule in that it also checks for multiple empty string positional arguments with an empty string separator.

Related to #1348.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-09-23 13:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @tjkuson on 2023-09-23 14:02_

---

_Comment by @codspeed-hq[bot] on 2023-09-23 14:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:print-empty-string)

### Merging #7617 will **not alter performance**

<sub>Comparing <code>tjkuson:print-empty-string</code> (8edeab2) with <code>main</code> (865c898)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @tjkuson on 2023-09-23 14:58_

I am unsure how much of the performance degradation can be attributed to having to check the arguments to every `print` call.

---

_Comment by @charliermarsh on 2023-09-23 16:04_

I think that regression is just noise (i.e., a false positive).

---

_Label `rule` added by @charliermarsh on 2023-09-24 03:46_

---

_Merged by @charliermarsh on 2023-09-24 04:10_

---

_Closed by @charliermarsh on 2023-09-24 04:10_

---

_Branch deleted on 2023-09-28 19:59_

---

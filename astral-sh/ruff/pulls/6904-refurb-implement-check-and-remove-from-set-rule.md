```yaml
number: 6904
title: "[refurb] Implement `check-and-remove-from-set` rule (`FURB132`)"
type: pull_request
state: merged
author: SavchenkoValeriy
labels:
  - rule
assignees: []
merged: true
base: main
head: refurb/furb132
created_at: 2023-08-26T20:32:08Z
updated_at: 2023-08-29T10:29:26Z
url: https://github.com/astral-sh/ruff/pull/6904
synced_at: 2026-01-12T15:55:22Z
```

# [refurb] Implement `check-and-remove-from-set` rule (`FURB132`)

---

_@SavchenkoValeriy_

## Summary

This PR is a continuation of #6897 and #6702  and me replicating `refurb` rules (#1348). It adds support for [FURB132](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/set_discard.py)

## Test Plan

I included a new test + checked that all other tests pass.

---

_Comment by @codspeed-hq[bot] on 2023-08-26 20:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/SavchenkoValeriy:refurb/furb132)

### Merging #6904 will **not alter performance**

<sub>Comparing <code>SavchenkoValeriy:refurb/furb132</code> (a4ffa8c) with <code>main</code> (eae59cf)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-08-26 20:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `rule` added by @charliermarsh on 2023-08-29 01:02_

---

_Merged by @charliermarsh on 2023-08-29 01:17_

---

_Closed by @charliermarsh on 2023-08-29 01:17_

---

_Branch deleted on 2023-08-29 10:29_

---

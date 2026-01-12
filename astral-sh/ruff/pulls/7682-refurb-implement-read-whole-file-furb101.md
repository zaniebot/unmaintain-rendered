```yaml
number: 7682
title: "[refurb] Implement `read-whole-file` [`FURB101`]"
type: pull_request
state: merged
author: SavchenkoValeriy
labels:
  - rule
assignees: []
merged: true
base: main
head: refurb/furb101
created_at: 2023-09-27T21:43:28Z
updated_at: 2023-10-20T16:29:45Z
url: https://github.com/astral-sh/ruff/pull/7682
synced_at: 2026-01-12T02:32:41Z
```

# [refurb] Implement `read-whole-file` [`FURB101`]

---

_Pull request opened by @SavchenkoValeriy on 2023-09-27 21:43_

## Summary

This PR is part of a bigger effort of re-implementing `refurb` rules #1348. It adds support for [FURB101](https://github.com/dosisod/refurb/blob/master/refurb/checks/pathlib/read_text.py)

## Test Plan

I included a new test + checked that all other tests pass.

---

_Comment by @codspeed-hq[bot] on 2023-09-27 21:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/SavchenkoValeriy:refurb/furb101)

### Merging #7682 will **not alter performance**

<sub>Comparing <code>SavchenkoValeriy:refurb/furb101</code> (87ea08d) with <code>main</code> (f158536)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-09-27 22:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @SavchenkoValeriy on 2023-09-30 18:22_

Hey @charliermarsh 👋 Do you mind taking a look at this PR? Thanks! 🙏 
PS I think that the performance test failure is a flake

---

_Comment by @charliermarsh on 2023-10-08 13:59_

Sorry, this is absolutely on my list, it's just a big rule implementation so trying to find time to focus on it.

---

_Comment by @SavchenkoValeriy on 2023-10-20 09:17_

@charliermarsh a gentle ping

---

_Comment by @charliermarsh on 2023-10-20 15:03_

@SavchenkoValeriy - Sorry for the delay, reading now!

---

_@charliermarsh approved on 2023-10-20 16:13_

---

_Comment by @SavchenkoValeriy on 2023-10-20 16:15_

Oh, really nice catch with argument unpacking! ❤️ 

---

_Comment by @charliermarsh on 2023-10-20 16:16_

@SavchenkoValeriy - I think there are a few cases we _could_ catch, but it felt like it didn't merit the complexity budget.

---

_Label `rule` added by @charliermarsh on 2023-10-20 16:16_

---

_Comment by @charliermarsh on 2023-10-20 16:16_

Excellent work @SavchenkoValeriy

---

_Merged by @charliermarsh on 2023-10-20 16:22_

---

_Closed by @charliermarsh on 2023-10-20 16:22_

---

_Branch deleted on 2023-10-20 16:29_

---

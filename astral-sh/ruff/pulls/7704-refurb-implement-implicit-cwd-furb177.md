```yaml
number: 7704
title: "[`refurb`] Implement `implicit-cwd` (FURB177)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
assignees: []
merged: true
base: main
head: no-implicit-cwd
created_at: 2023-09-28T21:16:21Z
updated_at: 2023-09-29T02:25:23Z
url: https://github.com/astral-sh/ruff/pull/7704
synced_at: 2026-01-12T02:39:10Z
```

# [`refurb`] Implement `implicit-cwd` (FURB177)

---

_Pull request opened by @danparizher on 2023-09-28 21:16_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement [`no-implicit-cwd`](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb177-no-implicit-cwd) as `implicit-cwd`

Related to #1348.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2023-09-28 22:08_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/danielparizher:no-implicit-cwd)

### Merging #7704 will **not alter performance**

<sub>Comparing <code>danielparizher:no-implicit-cwd</code> (755b6f2) with <code>main</code> (246d93e)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_@charliermarsh reviewed on 2023-09-29 01:32_

Only one change required here: can we rename from `NoImplicitCwd` to `ImplicitCwd`? And rename the file and function to `implicit_cwd`? Our convention is to describe the thing we're linting against, so e.g., you could say "allow implicit cwd".

---

_Label `rule` added by @charliermarsh on 2023-09-29 01:32_

---

_Renamed from "[`refurb`] Implement `no-implicit-cwd` (FURB177)" to "[`refurb`] Implement `implicit-cwd` (FURB177)" by @danparizher on 2023-09-29 01:38_

---

_Marked ready for review by @danparizher on 2023-09-29 01:40_

---

_@charliermarsh approved on 2023-09-29 02:09_

Thanks!

---

_Merged by @charliermarsh on 2023-09-29 02:19_

---

_Closed by @charliermarsh on 2023-09-29 02:19_

---

_Branch deleted on 2023-09-29 02:20_

---

_Comment by @github-actions[bot] on 2023-09-29 02:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

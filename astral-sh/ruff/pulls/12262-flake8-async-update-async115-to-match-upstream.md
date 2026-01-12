```yaml
number: 12262
title: "[`flake8-async`] Update `ASYNC115` to match upstream"
type: pull_request
state: merged
author: augustelalande
labels:
  - preview
assignees: []
merged: true
base: main
head: async115
created_at: 2024-07-10T02:24:14Z
updated_at: 2024-07-10T15:36:07Z
url: https://github.com/astral-sh/ruff/pull/12262
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-async`] Update `ASYNC115` to match upstream

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the name of `ASYNC115` to match [upstream](https://flake8-async.readthedocs.io/en/latest/rules.html).

Also update the functionality to match upstream by adding support for `anyio` (gated behind preview). `asyncio` does not have a `lowlevel.checkpoint` equivalent.

This resolves #12039.

## Test Plan

Added tests for `anyio`


---

_Comment by @codspeed-hq[bot] on 2024-07-10 02:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:async115)

### Merging #12262 will **not alter performance**

<sub>Comparing <code>augustelalande:async115</code> (f0ec230) with <code>main</code> (0bb2fc6)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Label `preview` added by @MichaReiser on 2024-07-10 07:10_

---

_@MichaReiser approved on 2024-07-10 07:14_

Nice, thank you. If I understand the PR correctly then it's also adding support for (`trio`|`anyio`)`.sleep`. We should gate this change behind preview, the same as for the other PRs.

---

_Comment by @MichaReiser on 2024-07-10 07:16_

> If I understand the PR correctly then it's also adding support for (trio|anyio).sleep. We should gate this change behind preview, the same as for the other PRs.

Sorry, I'm blind

---

_Merged by @MichaReiser on 2024-07-10 07:43_

---

_Closed by @MichaReiser on 2024-07-10 07:43_

---

_Branch deleted on 2024-07-10 15:36_

---

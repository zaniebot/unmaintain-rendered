```yaml
number: 19002
title: "[`flake8-async`] Make `ASYNC105` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-8
created_at: 2025-06-27T21:37:32Z
updated_at: 2025-06-30T18:37:10Z
url: https://github.com/astral-sh/ruff/pull/19002
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-async`] Make `ASYNC105` example error out-of-the-box

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [trio-sync-call (ASYNC105)](https://docs.astral.sh/ruff/rules/trio-sync-call/#trio-sync-call-async105)'s example error out-of-the-box

[Old example](https://play.ruff.rs/5b267e01-1c0a-4902-949e-45fc46f8b0d0)
```py
async def double_sleep(x):
    trio.sleep(2 * x)
```

[New example](https://play.ruff.rs/eba6ea40-ff88-4ea8-8cb4-cea472c15c53)
```py
import trio


async def double_sleep(x):
    trio.sleep(2 * x)
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 21:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @dylwil3 on 2025-06-28 15:17_

---

_@dylwil3 approved on 2025-06-28 15:17_

---

_Merged by @dylwil3 on 2025-06-28 15:18_

---

_Closed by @dylwil3 on 2025-06-28 15:18_

---

_Branch deleted on 2025-06-28 15:19_

---

_Branch restored on 2025-06-30 18:37_

---

_Branch deleted on 2025-06-30 18:37_

---

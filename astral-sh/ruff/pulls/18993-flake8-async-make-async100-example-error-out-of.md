```yaml
number: 18993
title: "[`flake8-async`] Make `ASYNC100` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-7
created_at: 2025-06-27T18:37:17Z
updated_at: 2025-06-28T15:20:51Z
url: https://github.com/astral-sh/ruff/pull/18993
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-async`] Make `ASYNC100` example error out-of-the-box

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

This PR makes [cancel-scope-no-checkpoint (ASYNC100)](https://docs.astral.sh/ruff/rules/cancel-scope-no-checkpoint/#cancel-scope-no-checkpoint-async100)'s example error out-of-the-box

[Old example](https://play.ruff.rs/6a399ae5-9b89-4438-b808-6604f1e40a70)
```py
async def func():
    async with asyncio.timeout(2):
        do_something()
```

[New example](https://play.ruff.rs/c44db531-d2f8-4a61-9e04-e5fc0ea989e3)
```py
import asyncio


async def func():
    async with asyncio.timeout(2):
        do_something()
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 18:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-28 15:13_

---

_Label `documentation` added by @dylwil3 on 2025-06-28 15:13_

---

_Merged by @dylwil3 on 2025-06-28 15:13_

---

_Closed by @dylwil3 on 2025-06-28 15:13_

---

_Branch deleted on 2025-06-28 15:20_

---

```yaml
number: 18975
title: "[`flake8-async`] Make `ASYNC110` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-27T04:09:37Z
updated_at: 2025-06-27T07:03:31Z
url: https://github.com/astral-sh/ruff/pull/18975
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-async`] Make `ASYNC110` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-27 04:09_

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

This PR makes [async-busy-wait (ASYNC110)](https://docs.astral.sh/ruff/rules/async-busy-wait/#async-busy-wait-async110)'s example error out-of-the-box

[Old example](https://play.ruff.rs/29e675b6-cd48-4208-8d4f-1c3a7b4d85c9)
```py
DONE = False


async def func():
    while not DONE:
        await asyncio.sleep(1)
```

[New example](https://play.ruff.rs/d21c27b5-fdfd-46f1-ae30-5f8c1399d936)
```py
import asyncio

DONE = False

async def func():
    while not DONE:
        await asyncio.sleep(1)
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 04:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @MichaReiser on 2025-06-27 07:01_

---

_Merged by @MichaReiser on 2025-06-27 07:01_

---

_Closed by @MichaReiser on 2025-06-27 07:01_

---

_Branch deleted on 2025-06-27 07:03_

---

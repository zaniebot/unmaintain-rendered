```yaml
number: 18990
title: "[`flake8-async`] Make `ASYNC251` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-6
created_at: 2025-06-27T17:56:23Z
updated_at: 2025-06-28T15:20:45Z
url: https://github.com/astral-sh/ruff/pull/18990
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-async`] Make `ASYNC251` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-27 17:56_

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

This PR makes [blocking-sleep-in-async-function (ASYNC251)](https://docs.astral.sh/ruff/rules/blocking-sleep-in-async-function/#blocking-sleep-in-async-function-async251)'s example error out-of-the-box

[Old example](https://play.ruff.rs/796684a2-c437-4390-b754-491e576ffe5e)
```py
async def fetch():
    time.sleep(1)
```

[New example](https://play.ruff.rs/90741192-fd0d-49fb-a04e-3127312da659)
```py
import time


async def fetch():
    time.sleep(1)
```

Imports were also added to the `Use instead:` section to make it valid code out-of-the-box.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 18:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @dylwil3 on 2025-06-28 15:15_

---

_@dylwil3 approved on 2025-06-28 15:15_

---

_Merged by @dylwil3 on 2025-06-28 15:15_

---

_Closed by @dylwil3 on 2025-06-28 15:15_

---

_Branch deleted on 2025-06-28 15:20_

---

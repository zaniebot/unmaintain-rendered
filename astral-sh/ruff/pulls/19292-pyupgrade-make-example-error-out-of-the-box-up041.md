```yaml
number: 19292
title: "[`pyupgrade`] Make example error out-of-the-box (`UP041`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-07-11T21:05:04Z
updated_at: 2025-07-11T21:15:50Z
url: https://github.com/astral-sh/ruff/pull/19292
synced_at: 2026-01-10T18:33:12Z
```

# [`pyupgrade`] Make example error out-of-the-box (`UP041`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-11 21:05_

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

This PR makes [timeout-error-alias (UP041)](https://docs.astral.sh/ruff/rules/timeout-error-alias/#timeout-error-alias-up041)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/87e20352-d80a-46ec-98a2-6f6ea700438b)
```py
raise asyncio.TimeoutError
```

[New example](https://play.ruff.rs/d3b95557-46a2-4856-bd71-30d5f3f5ca44)
```py
import asyncio

raise asyncio.TimeoutError
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_@dylwil3 approved on 2025-07-11 21:08_

---

_Label `documentation` added by @dylwil3 on 2025-07-11 21:08_

---

_Merged by @dylwil3 on 2025-07-11 21:08_

---

_Closed by @dylwil3 on 2025-07-11 21:08_

---

_Branch deleted on 2025-07-11 21:10_

---

_Comment by @github-actions[bot] on 2025-07-11 21:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

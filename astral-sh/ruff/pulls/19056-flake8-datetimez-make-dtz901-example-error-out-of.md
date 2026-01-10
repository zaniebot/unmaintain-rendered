```yaml
number: 19056
title: "[`flake8-datetimez`] Make `DTZ901` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-30T21:24:35Z
updated_at: 2025-07-01T22:25:02Z
url: https://github.com/astral-sh/ruff/pull/19056
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-datetimez`] Make `DTZ901` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-30 21:24_

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

This PR makes [datetime-min-max (DTZ901)](https://docs.astral.sh/ruff/rules/datetime-min-max/#datetime-min-max-dtz901)'s example error out-of-the-box

[Old example](https://play.ruff.rs/c1202727-1a18-4d3f-92a4-334ede07ed3e)
```py
datetime.max
```

[New example](https://play.ruff.rs/af2c76aa-9beb-46bc-8e27-faf53ecdbe8c)
```py
import datetime

datetime.datetime.max
```

I also added imports to the problem demonstration and use instead.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-30 21:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-01 13:57_

Thanks!

---

_Merged by @ntBre on 2025-07-01 13:57_

---

_Closed by @ntBre on 2025-07-01 13:57_

---

_Label `documentation` added by @ntBre on 2025-07-01 13:57_

---

_Branch deleted on 2025-07-01 22:25_

---

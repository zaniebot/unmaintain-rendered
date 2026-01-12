```yaml
number: 19288
title: "[`pylint`] Make example error out-of-the-box (`PLE1507`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2025-07-11T17:49:48Z
updated_at: 2025-07-11T21:09:55Z
url: https://github.com/astral-sh/ruff/pull/19288
synced_at: 2026-01-12T15:56:36Z
```

# [`pylint`] Make example error out-of-the-box (`PLE1507`)

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

This PR makes [invalid-envvar-value (PLE1507)](https://docs.astral.sh/ruff/rules/invalid-envvar-value/#invalid-envvar-value-ple1507)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/a46a9bca-edd5-4474-b20d-e6b6d87291ca)
```py
os.getenv(1)
```

[New example](https://play.ruff.rs/8348d32d-71fa-422c-b228-e2bc343765b1)
```py
import os

os.getenv(1)
```

The "Use instead" section was also updated similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-11 18:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-07-11 21:08_

---

_Merged by @dylwil3 on 2025-07-11 21:08_

---

_Closed by @dylwil3 on 2025-07-11 21:08_

---

_Label `documentation` added by @dylwil3 on 2025-07-11 21:08_

---

_Branch deleted on 2025-07-11 21:09_

---

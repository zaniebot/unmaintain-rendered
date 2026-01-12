```yaml
number: 19191
title: "[`pycodestyle`] Make example error out-of-the-box (`E272`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-07-07T23:00:07Z
updated_at: 2025-07-08T14:34:51Z
url: https://github.com/astral-sh/ruff/pull/19191
synced_at: 2026-01-12T15:56:33Z
```

# [`pycodestyle`] Make example error out-of-the-box (`E272`)

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

This PR makes [multiple-spaces-before-keyword (E272)](https://docs.astral.sh/ruff/rules/multiple-spaces-before-keyword/#multiple-spaces-before-keyword-e272)'s example error out-of-the-box. Since `True` is also a keyword, the old example raises `E271` instead.

[Old example](https://play.ruff.rs/23ec3774-5038-471c-be3f-1c1e36f85cbb)
```py
True  and False
```

[New example](https://play.ruff.rs/d77432e2-fd99-4db2-9cd0-bc08675c0aca)
```py
x  and y
```

The "Use instead" section was also updated similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-07 23:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-08 02:31_

---

_Label `documentation` added by @ntBre on 2025-07-08 02:31_

---

_@ntBre approved on 2025-07-08 13:57_

---

_Merged by @ntBre on 2025-07-08 13:58_

---

_Closed by @ntBre on 2025-07-08 13:58_

---

_Branch deleted on 2025-07-08 14:34_

---

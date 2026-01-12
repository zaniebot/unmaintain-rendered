```yaml
number: 19672
title: "[`refurb`] Make example error out-of-the-box (`FURB180`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-31T20:58:03Z
updated_at: 2025-07-31T21:28:30Z
url: https://github.com/astral-sh/ruff/pull/19672
synced_at: 2026-01-12T15:56:44Z
```

# [`refurb`] Make example error out-of-the-box (`FURB180`)

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

This PR makes [meta-class-abc-meta (FURB180)](https://docs.astral.sh/ruff/rules/meta-class-abc-meta/#meta-class-abc-meta-furb180)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/6beca1be-45cd-4e5a-aafa-6a0584c10d64)
```py
class C(metaclass=ABCMeta):
    pass
```

[New example](https://play.ruff.rs/bbad34da-bf07-44e6-9f34-53337e8f57d4)
```py
import abc


class C(metaclass=abc.ABCMeta):
    pass
```

The "Use instead" section as also modified similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Label `documentation` added by @ntBre on 2025-07-31 21:07_

---

_@ntBre approved on 2025-07-31 21:07_

Thank you!

---

_Comment by @github-actions[bot] on 2025-07-31 21:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-07-31 21:14_

---

_Closed by @ntBre on 2025-07-31 21:14_

---

_Branch deleted on 2025-07-31 21:28_

---

```yaml
number: 19106
title: "[`flake8-quotes`] Make example error out-of-the-box (`Q003`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-07-02T22:16:07Z
updated_at: 2025-07-03T15:54:03Z
url: https://github.com/astral-sh/ruff/pull/19106
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-quotes`] Make example error out-of-the-box (`Q003`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-02 22:16_

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

This PR makes [avoidable-escaped-quote (Q003)](https://docs.astral.sh/ruff/rules/avoidable-escaped-quote/#avoidable-escaped-quote-q003)'s example error out-of-the-box

[Old example](https://play.ruff.rs/fb319d0f-8016-46a1-b6bb-42b1b054feea)
```py
foo = 'bar\'s'
```

[New example](https://play.ruff.rs/d9626561-0646-448f-9282-3f0691b90831)
```py
foo = "bar\"s"
```

The original example got overwritten by `Q000`, since double quotes is the default config. The quotes were also switched in the "Use instead" section.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-02 22:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-03 14:25_

---

_Label `documentation` added by @ntBre on 2025-07-03 14:25_

---

_Merged by @ntBre on 2025-07-03 14:25_

---

_Closed by @ntBre on 2025-07-03 14:25_

---

_Branch deleted on 2025-07-03 15:54_

---

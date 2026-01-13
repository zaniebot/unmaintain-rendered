```yaml
number: 22558
title: "[`ruff`] Make example error out-of-the-box (`RUF103`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2026-01-13T19:24:41Z
updated_at: 2026-01-13T20:06:55Z
url: https://github.com/astral-sh/ruff/pull/22558
synced_at: 2026-01-13T20:37:04Z
```

# [`ruff`] Make example error out-of-the-box (`RUF103`)

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

This PR makes [invalid-suppression-comment (RUF103)](https://docs.astral.sh/ruff/rules/invalid-suppression-comment/#invalid-suppression-comment-ruf103)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/3ff757f3-04ae-4d27-986d-49972338fa24)
```py
ruff: disable # missing codes
```

[New example](https://play.ruff.rs/4a9970c4-3b33-4533-8ffa-f15d481b1e6f)
```py
# ruff: disable # missing codes
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 19:38_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `documentation` added by @ntBre on 2026-01-13 20:05_

---

_@ntBre approved on 2026-01-13 20:06_

Thank you!

---

_Renamed from "[`RUF`] Make example error out-of-the-box (`RUF103`)" to "[`ruff`] Make example error out-of-the-box (`RUF103`)" by @ntBre on 2026-01-13 20:06_

---

_Merged by @ntBre on 2026-01-13 20:06_

---

_Closed by @ntBre on 2026-01-13 20:06_

---

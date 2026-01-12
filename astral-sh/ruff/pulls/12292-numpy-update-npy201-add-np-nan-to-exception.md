```yaml
number: 12292
title: "[numpy] Update NPY201: add `np.NAN` to exception"
type: pull_request
state: merged
author: xmatthias
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/npy201_NAN
created_at: 2024-07-11T18:40:56Z
updated_at: 2024-07-12T12:53:06Z
url: https://github.com/astral-sh/ruff/pull/12292
synced_at: 2026-01-12T15:55:40Z
```

# [numpy] Update NPY201: add `np.NAN` to exception

---

_@xmatthias_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

np.NAN was also deprecated / removed, as reported in #12195.


Personally, i'd also wan't to have warnings for `from numpy import nan, NaN, NAN` (NAN and NaN shouldn't import as they've been removed) - but that's really out of my league - as there's no similar case at the moment (at least not in the NPY201 section).
Obviously, this would "also" apply to essentially all other cases of deprecations.

## Test Plan

Added testcase, manual test

---

_Review comment by @xmatthias on `crates/ruff_linter/resources/test/fixtures/numpy/NPY201.py`:72 on 2024-07-11 18:47_

this could also be moved next to `np.NaN` - which i'd find more appealing as it'd really belong there - but i found the diff on the .snap file to be rather large - as it changes all subsequent line numbers... 

let me know if you'd prefer that

---

_@xmatthias reviewed on 2024-07-11 18:47_

---

_@MichaReiser reviewed on 2024-07-12 12:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/numpy/NPY201.py`:72 on 2024-07-12 12:04_

I think either is fine. 

---

_Comment by @MichaReiser on 2024-07-12 12:04_

> Personally, i'd also wan't to have warnings for from numpy import nan, NaN, NAN (NAN and NaN shouldn't import as they've been removed) - but that's really out of my league - as there's no similar case at the moment (at least not in the NPY201 section).

That makes sense. I think that's best tackled as a separate pr

---

_Label `rule` added by @MichaReiser on 2024-07-12 12:05_

---

_Renamed from "[numpy] Update NPY201: add np.NAN to exception" to "[numpy] Update NPY201: add `np.NAN` to exception" by @MichaReiser on 2024-07-12 12:05_

---

_Comment by @MichaReiser on 2024-07-12 12:06_

Thank you!

---

_Merged by @MichaReiser on 2024-07-12 12:09_

---

_Closed by @MichaReiser on 2024-07-12 12:09_

---

_Branch deleted on 2024-07-12 12:53_

---

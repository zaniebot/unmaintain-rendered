```yaml
number: 18203
title: "[ty] Add support for PyPy virtual environments"
type: pull_request
state: merged
author: Mathemmagician
labels:
  - ty
assignees: []
merged: true
base: main
head: better-site-packages-lookup
created_at: 2025-05-19T17:55:50Z
updated_at: 2025-05-20T19:16:26Z
url: https://github.com/astral-sh/ruff/pull/18203
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Add support for PyPy virtual environments

---

_Pull request opened by @Mathemmagician on 2025-05-19 17:55_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Allows for better package discovery, ex. PyPy (fixes https://github.com/astral-sh/ty/issues/450)

Instead of only looking for `site-packages` in `lib/pythonX.Y/`, we also look into `lib/pypyX.Y/`. To avoid guesswork, we look at the `Implementation` value provided by virtual environments inside `pyvenv.cfg`. 


- CPython or GraalPy -> `lib/pythonX.Y/site-packages`
- Pypy -> `lib/pypyX.Y/site-packages`
- Missing/Other value? -> Check both locations


## Test Plan

<!-- How was it tested? -->

As of now, passed all current tests, but no new tests have been added yet.

TODO: 
1. Add new tests, i.e., adding Implementation field (PyPy vs CPython vs other vs missing) to VirtualEnvironmentTestCase
2. Update comments/docstrings


---

_Label `ty` added by @ntBre on 2025-05-19 17:58_

---

_Comment by @Mathemmagician on 2025-05-19 17:59_

Thank you, @AlexWaygood, for all the help and a super friendly first PyCon sprint experience :)

---

_Comment by @github-actions[bot] on 2025-05-19 18:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "first pass adding implementation flag" to "[ty] Add support for PyPy virtual environments" by @AlexWaygood on 2025-05-19 22:03_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-19 22:03_

---

_Comment by @AlexWaygood on 2025-05-19 22:04_

I fixed CI and pushed a couple of nits. I'll add a test tomorrow and land it -- this looks great, thank you!!

---

_Comment by @AlexWaygood on 2025-05-20 18:28_

I added some unit tests to `site_packages.rs`. Ideally we'd add some mdtests here as well, but that would require adding some new features to mdtest to handle PyPy venv layouts, and that doesn't really seem worth it here.

---

_Marked ready for review by @AlexWaygood on 2025-05-20 18:32_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-20 18:32_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2025-05-20 18:32_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-20 18:32_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-20 18:32_

---

_@carljm approved on 2025-05-20 18:44_

Looks good!

---

_Merged by @AlexWaygood on 2025-05-20 18:46_

---

_Closed by @AlexWaygood on 2025-05-20 18:46_

---

_Comment by @AlexWaygood on 2025-05-20 18:47_

Thank you @Mathemmagician!!

---

_Comment by @Mathemmagician on 2025-05-20 19:16_

Thanks @AlexWaygood !!! 🥳

---

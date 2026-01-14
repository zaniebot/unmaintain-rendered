```yaml
number: 22565
title: "FAST002 : add autofix skipped reason as diagnostic info"
type: pull_request
state: open
author: 11happy
labels:
  - diagnostics
assignees: []
base: main
head: Extend_FAST002
created_at: 2026-01-14T09:00:25Z
updated_at: 2026-01-14T20:57:36Z
url: https://github.com/astral-sh/ruff/pull/22565
synced_at: 2026-01-14T21:43:07Z
```

# FAST002 : add autofix skipped reason as diagnostic info

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes #22188 by adding autofix skipped reason as diagnostic info.

## Test Plan

<!-- How was it tested? -->
snapshots are updated accordingly.


---

_Label `diagnostics` added by @MichaReiser on 2026-01-14 09:03_

---

_@MichaReiser reviewed on 2026-01-14 09:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_0.py_py38.snap`:361 on 2026-01-14 09:07_

I find the messaging here a bit confusing. It almost sounds as if there's no way to fix this violation (I'm also not fully convinced that we should add a sub-diagnostic like this, or that we should only show it when running ruff with `-v`).

Maybe something like: `info: automatic fix is not available because`



---

_Review comment by @11happy on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_0.py_py38.snap`:361 on 2026-01-14 09:09_

yeah, I agree these can be worded better, let me update.

---

_@11happy reviewed on 2026-01-14 09:09_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 09:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@11happy reviewed on 2026-01-14 09:13_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_0.py_py38.snap`:361 on 2026-01-14 09:13_

Done, changed the messages to be a little bit more verbose & user friendly.

---

_@11happy reviewed on 2026-01-14 09:17_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_0.py_py38.snap`:361 on 2026-01-14 09:17_

I am also leaning towards making these subdiagnostics available in case of  verbose mode(-v)

---

_@ntBre reviewed on 2026-01-14 20:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_0.py_py38.snap`:361 on 2026-01-14 20:57_

Hmm, yeah maybe this wasn't a great suggestion for a sub-diagnostic, sorry! We could also consider just adding a `Fix availability` note to the documentation instead.

Alternatively, it might be more helpful to phrase the sub-diagnostic as a suggestion like "Reorder the arguments to enable an automatic fix" (but probably more specific than that).

Just a couple of ideas, I'm curious what y'all think.

---

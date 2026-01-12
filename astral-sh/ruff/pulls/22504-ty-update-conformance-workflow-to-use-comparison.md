```yaml
number: 22504
title: "[ty] Update conformance workflow to use comparison script"
type: pull_request
state: open
author: WillDuke
labels:
  - ci
  - ty
assignees: []
base: main
head: wld/update-conformance-workflow
created_at: 2026-01-11T17:55:59Z
updated_at: 2026-01-12T18:46:29Z
url: https://github.com/astral-sh/ruff/pull/22504
synced_at: 2026-01-12T19:26:24Z
```

# [ty] Update conformance workflow to use comparison script

---

_@WillDuke_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:



- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR updates the typing conformance workflow to use the `conformance.py` script added in #22231.

Partially addresses  [#2205](https://github.com/astral-sh/ty/issues/2205).

## Test Plan

Reviewing CI result once #22231 merges. 

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-11 18:07_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Label `ci` added by @AlexWaygood on 2026-01-12 08:25_

---

_Label `ty` added by @AlexWaygood on 2026-01-12 08:25_

---

_Comment by @MichaReiser on 2026-01-12 17:32_

@WillDuke The CI job is currently failing because it can't find the file. Would you mind taking a look or would you prefer one of us doing the CI integration?

---

_Comment by @WillDuke on 2026-01-12 18:46_

> @WillDuke The CI job is currently failing because it can't find the file. Would you mind taking a look or would you prefer one of us doing the CI integration?

CI is failing since `scripts/conformance.py` does not yet exist on main. Once #22231 merges, I can close and reopen this to check that the job starts working.

---

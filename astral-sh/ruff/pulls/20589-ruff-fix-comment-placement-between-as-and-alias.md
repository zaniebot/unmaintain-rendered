```yaml
number: 20589
title: "[`ruff`] Fix comment placement between as and alias name in from import"
type: pull_request
state: closed
author: TaKO8Ki
labels: []
assignees: []
base: main
head: formatter-import-from-alias-dangling
created_at: 2025-09-26T05:07:44Z
updated_at: 2025-09-26T06:47:07Z
url: https://github.com/astral-sh/ruff/pull/20589
synced_at: 2026-01-12T15:57:05Z
```

# [`ruff`] Fix comment placement between as and alias name in from import

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19138

- Treat comments located between the as token and the alias name as trailing comments on the alias.
- Add regression test to cover the case.

## Test Plan

<!-- How was it tested? -->

I have added a test case to import_from.py.


---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-09-26 05:07_

---

_Comment by @github-actions[bot] on 2025-09-26 05:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-09-26 06:45_

Uff sorry. @amyreese was already [working on this](https://github.com/astral-sh/ruff/pull/20561) when we assigned the issue to you. We didn't notice because I forgot to assign @amyreese. I'm very sorry that this happened.

I close this in favor of https://github.com/astral-sh/ruff/pull/20561. 

---

_Closed by @MichaReiser on 2025-09-26 06:45_

---

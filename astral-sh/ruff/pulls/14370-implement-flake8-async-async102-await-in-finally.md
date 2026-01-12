```yaml
number: 14370
title: implement flake8_async ASYNC102 await in finally or cancelled
type: pull_request
state: open
author: altendky
labels:
  - rule
  - preview
assignees: []
draft: true
base: main
head: add_unshielded_await
created_at: 2024-11-15T21:58:34Z
updated_at: 2025-01-20T15:02:15Z
url: https://github.com/astral-sh/ruff/pull/14370
synced_at: 2026-01-12T15:55:47Z
```

# implement flake8_async ASYNC102 await in finally or cancelled

---

_@altendky_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When working with async cleanup activities it is important to be aware of shielding the cleanup from cancellation.

https://anyio.readthedocs.io/en/stable/cancellation.html#shielding

This rule looks for unshielded awaits in relevant cleanup paths such as exception handling and finally blocks.  It walks the AST looking for awaits but prunes when it finds shielding context managers.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

## Draft For:
- [ ] review `TODO`s
- [x] consider if this is the ruff implementation of https://flake8-async.readthedocs.io/en/latest/rules.html#async102 (at a glance the answer seems to be yes)
- [x] explicitly test async for and async with
- [ ] review `# noqa: ASYNC102 - fixthis` which are mostly around direct assignment to the `.shield` attribute
- [x] revisit linter message, it's just `shield it!` right now
- [x] don't trigger on `anyio` and `trio.aclose_forcefully()`
- [x] review all panics and either implement or handle by having no output
- [x] handle `.__aexit__()`
- [x] require a ~timeout~ deadline

---

_Comment by @github-actions[bot] on 2024-11-15 22:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "very preliminary unshielded await rule" to "somewhat preliminary unshielded await rule" by @altendky on 2024-11-18 15:39_

---

_Renamed from "somewhat preliminary unshielded await rule" to "somewhat preliminary unshielded async rule" by @altendky on 2024-11-18 15:39_

---

_Renamed from "somewhat preliminary unshielded async rule" to "implement flake8_async ASYNC102 await in finally or cancelled" by @altendky on 2024-11-18 18:39_

---

_Label `rule` added by @dhruvmanila on 2025-01-20 15:02_

---

_Label `preview` added by @dhruvmanila on 2025-01-20 15:02_

---

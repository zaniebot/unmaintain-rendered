```yaml
number: 13300
title: "[`pydoclint`] Ignore `DOC201` when function name is \"__new__\""
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: doc201
created_at: 2024-09-10T04:08:57Z
updated_at: 2024-09-10T17:35:06Z
url: https://github.com/astral-sh/ruff/pull/13300
synced_at: 2026-01-10T21:38:32Z
```

# [`pydoclint`] Ignore `DOC201` when function name is "__new__"

---

_Pull request opened by @augustelalande on 2024-09-10 04:08_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #13079.

Do not enforce the documentation of `return` in a `__new__` function. Arguably this could apply to some other dunder methods.

## Test Plan

Added fixture.


---

_Comment by @github-actions[bot] on 2024-09-10 04:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-09-10 13:33_

It is actually possible for `__new__` methods to not return an instance of the class (e.g. https://github.com/python/cpython/blob/962304a54ca79da0838cf46dd4fb744045167cdd/Lib/pathlib/_local.py#L521-L524). But I agree that that's something of a rare edge case. We could in theory do some static analysis to figure out if `__new__` returns an instance of the current class or not, but I think it's probably not worth it with our current type inference capabilities. So this LGTM.

I do also agree that we could probably extend it to some other dunders as well -- probably all of these?

https://github.com/astral-sh/ruff/blob/7c872e639bf2e657113d16a5f98b66590deba7ea/crates/ruff_python_stdlib/src/typing.rs#L355-L370

---

_Label `rule` added by @MichaReiser on 2024-09-10 16:26_

---

_Label `preview` added by @MichaReiser on 2024-09-10 16:26_

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-09-10 16:26_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-10 17:24_

---

_Merged by @AlexWaygood on 2024-09-10 17:25_

---

_Closed by @AlexWaygood on 2024-09-10 17:25_

---

_Comment by @AlexWaygood on 2024-09-10 17:26_

> I do also agree that we could probably extend it to some other dunders as well -- probably all of these?

(These can be done as a followup, so I merged this for now :-)

---

_Branch deleted on 2024-09-10 17:35_

---

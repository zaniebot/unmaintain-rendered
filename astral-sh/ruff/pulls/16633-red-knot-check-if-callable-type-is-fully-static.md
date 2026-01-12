```yaml
number: 16633
title: "[red-knot] Check if callable type is fully static"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-is-fully-static
created_at: 2025-03-11T16:17:21Z
updated_at: 2025-03-12T06:43:24Z
url: https://github.com/astral-sh/ruff/pull/16633
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Check if callable type is fully static

---

_@dhruvmanila_

## Summary

Part of #15382 

This PR adds the check for whether a callable type is fully static or not.

A callable type is fully static if all of the parameter types are fully static _and_ the return type is fully static _and_ if it does not use the gradual form (`...`) for its parameters.

## Test Plan

Update `is_fully_static.md` with callable types.

It seems that currently this test is grouped into either fully static or not, I think it would be useful to split them up in groups like callable, etc. I intentionally avoided that in this PR but I'll put up a PR for an appropriate split.

Note: I've an explicit goal of updating the property tests with the new callable types once all relations are implemented.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-11 16:17_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-11 16:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-11 16:17_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-11 16:17_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-11 16:17_

---

_Comment by @github-actions[bot] on 2025-03-11 16:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-03-11 19:01_

This looks good, thank you.

May I ask why you plan to wait with the property tests until after you implemented all type properties? It seems to me like they could be of help. If you start out by returning `false` everywhere, they might even pass most existing property tests everywhere since they wouldn't fulfill the preconditions?

---

_@carljm approved on 2025-03-12 04:18_

---

_Comment by @dhruvmanila on 2025-03-12 06:07_

> May I ask why you plan to wait with the property tests until after you implemented all type properties? It seems to me like they could be of help. If you start out by returning `false` everywhere, they might even pass most existing property tests everywhere since they wouldn't fulfill the preconditions?

Not at all. My main reason was to independently put up PRs for each method and continue with the others without blocking the ones that are good to go and enabling property tests _could_ create CI failures on `main` for the ones that haven't been implemented yet.

That said, I do use a default return value (mostly `false`) for the ones that haven't been implemented yet and I can try adding them to verify today. Thanks for the suggestion.

---

_Merged by @dhruvmanila on 2025-03-12 06:43_

---

_Closed by @dhruvmanila on 2025-03-12 06:43_

---

_Branch deleted on 2025-03-12 06:43_

---

```yaml
number: 20516
title: "[ty] Enum narrowing eq ne"
type: pull_request
state: closed
author: Guillaume-Fgt
labels:
  - ty
assignees: []
base: main
head: enum_narrowing_eq_ne
created_at: 2025-09-22T14:51:42Z
updated_at: 2025-09-26T11:33:06Z
url: https://github.com/astral-sh/ruff/pull/20516
synced_at: 2026-01-12T15:57:03Z
```

# [ty] Enum narrowing eq ne

---

_@Guillaume-Fgt_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

In response to https://github.com/astral-sh/ty/issues/1177. This is my attempt at trying to modify the narrow mechanism of enum.

I implemented it this way because I didn't want to alter `fn is_single_valued` signature as it is used in 103 different places. Instead, I created a `fn is_single_valued_enum` and an optional parameter to modify if necessary the dunder methods checked by `fn overrides_equality`.

## Test Plan
I added a test with the case exposed in the issue mentioned above.


---

_Review requested from @carljm by @Guillaume-Fgt on 2025-09-22 14:51_

---

_Review requested from @AlexWaygood by @Guillaume-Fgt on 2025-09-22 14:51_

---

_Review requested from @sharkdp by @Guillaume-Fgt on 2025-09-22 14:51_

---

_Review requested from @dcreager by @Guillaume-Fgt on 2025-09-22 14:51_

---

_Comment by @github-actions[bot] on 2025-09-22 14:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @AlexWaygood on 2025-09-22 15:03_

---

_Comment by @github-actions[bot] on 2025-09-22 15:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Assigned to @sharkdp by @sharkdp on 2025-09-23 10:10_

---

_@sharkdp requested changes on 2025-09-25 08:34_

Thank you for your contribution.

To be honest, looking at the increased code complexity here, I'm not sure if it's even worth adding this functionality, for a use case that seems extremely unlikely.

If we want to move forward with this, I think we should aim to find a less invasive way to implement it. I find the meaning of the second parameter to `is_single_valued_enum` unclear (what does it mean if I pass in `Some(["__eq__"])`? What does it mean if I pass in `Some([])`? What does it mean if I pass in `None`?).

There are also a lot of `&Vec<…>`  parameters here which could be `&[…]` *if* we keep this implementation. I also see a lot of `.clone()`s that could probably be prevented.

I'm sorry that this is a rather negative feedback. I probably shouldn't have labeled https://github.com/astral-sh/ruff/pull/20516 as a good-first-issue, given that it wasn't even clear if it was worth implementing this at all.

---

_Comment by @Guillaume-Fgt on 2025-09-26 11:33_

Thank you for your review. I understand the reservations you made. I close the PR.

---

_Closed by @Guillaume-Fgt on 2025-09-26 11:33_

---

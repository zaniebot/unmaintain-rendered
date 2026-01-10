```yaml
number: 19275
title: "[ty] Pretty-format overloaded functions/methods/callables in hover"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - ty
assignees: []
draft: true
base: main
head: ty-overloads-display
created_at: 2025-07-11T06:10:17Z
updated_at: 2025-07-28T07:39:02Z
url: https://github.com/astral-sh/ruff/pull/19275
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Pretty-format overloaded functions/methods/callables in hover

---

_Pull request opened by @InSyncWithFoo on 2025-07-11 06:10_

## Summary

<i>This PR's description is currently empty.</i>

## Test Plan

Markdown tests and unit tests.



---

_Comment by @github-actions[bot] on 2025-07-11 06:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-07-11 08:20_

---

_Comment by @MichaReiser on 2025-07-11 16:02_

This is great. Before you go to deep on this. Can you tell us a bit about where you're getting inspiration from for the formatting (e.g. do you do what pylance does?). I just want to avoid that we go too deep before we're aligned on a rought formatting. If the goal of this PR is to play with different formattings, than this is completely fine. 

---

_Comment by @InSyncWithFoo on 2025-07-11 16:26_

@MichaReiser This PR was submitted primarily for testing (I didn't have a concrete proposal at that point). I'll file an issue and we can discuss there tomorrow.

---

_Comment by @MichaReiser on 2025-07-11 16:28_

That makes sense. Thank you.

---

_Comment by @InSyncWithFoo on 2025-07-11 16:41_

(Actually, it seems like there have already been one at [#102](https://github.com/astral-sh/ty/issues/102).)

---

_Review comment by @T-256 on `crates/ty_ide/src/hover.rs`:884 on 2025-07-11 20:37_

what's nicer to me:

```python
        (bound method of "<class 'C'>")
        @overload
        def f(v: int) -> int: ...
        @overload
        def f(v: str) -> str: ...
        ```


---

_@T-256 reviewed on 2025-07-11 20:41_

---

_@InSyncWithFoo reviewed on 2025-07-12 00:29_

---

_Review comment by @InSyncWithFoo on `crates/ty_ide/src/hover.rs`:884 on 2025-07-12 00:29_

This format is already used for single-signature callables, and I have to say I prefer it over yours, especially when the instance is of a specialized generic type:

```python
def C[int].f(v: int) -> int: ...
```

...but let's bring this to [#102](https://github.com/astral-sh/ty/issues/102).

---

_Comment by @github-actions[bot] on 2025-07-28 07:37_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

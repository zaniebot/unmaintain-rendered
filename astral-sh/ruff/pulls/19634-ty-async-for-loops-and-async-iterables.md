```yaml
number: 19634
title: "[ty] Async for loops and async iterables"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/async-for
created_at: 2025-07-30T11:31:46Z
updated_at: 2025-07-30T15:40:26Z
url: https://github.com/astral-sh/ruff/pull/19634
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Async for loops and async iterables

---

_Pull request opened by @sharkdp on 2025-07-30 11:31_

## Summary

Add support for `async for` loops and async iterables.

part of https://github.com/astral-sh/ty/issues/151

## Ecosystem impact

```diff
- boostedblob/listing.py:445:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
```

This is correct. We now find a true positive in the `# type: ignore`'d code.

All of the other ecosystem hits are of the type

```diff
trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_guest_mode.py:532:24: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be iterable
```

The message is correct, because only `MemoryReceiveChannel` has an `__aiter__` method, but `MemorySendChannel` does not. What's not correct is our inferred type here. It should be `MemoryReceiveChannel[int]`, not the union of the two. This is due to missing unpacking support for tuple subclasses, which @AlexWaygood is working on. I don't think this should block merging this PR, because those wrong types are already there, without this PR.

## Test Plan

New Markdown tests and snapshot tests for diagnostics.

---

_Label `ty` added by @sharkdp on 2025-07-30 11:31_

---

_Comment by @github-actions[bot] on 2025-07-30 11:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-30 11:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_trio.py:909:47: error[not-iterable] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` may not be async-iterable
- Found 85 diagnostics
+ Found 86 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/middleware/base.py:165:38: error[not-iterable] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` may not be async-iterable
- Found 161 diagnostics
+ Found 162 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_guest_mode.py:532:24: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
+ src/trio/_tests/test_channel.py:86:41: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
+ src/trio/_tests/test_channel.py:308:29: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- Found 734 diagnostics
+ Found 737 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/listing.py:445:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 26 diagnostics
+ Found 25 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-30 13:46_

---

_Comment by @github-actions[bot] on 2025-07-30 13:55_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `not-iterable` | 5 | 0 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **5** | **1** | **0** |

**[Full report with detailed diff](https://david-async-for.ecosystem-663.pages.dev/diff)**


---

_@sharkdp reviewed on 2025-07-30 14:02_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_Possibly_invalid_`__…_(c626bde8651b643a).snap`:63 on 2025-07-30 14:02_

Small drive-by fix.

---

_Marked ready for review by @sharkdp on 2025-07-30 14:33_

---

_Review requested from @carljm by @sharkdp on 2025-07-30 14:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-30 14:33_

---

_Review requested from @dcreager by @sharkdp on 2025-07-30 14:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/comprehensions/basic.md`:150 on 2025-07-30 14:46_

hmm, can't we be a bit more confident here in our language? And also more precise (in that it _is_ iterable, but it's not _async_-iterable, and _async_ iterability is what's required in this case)

```suggestion
    # error: [not-iterable] "Object of type `Iterable` is not async-iterable"
```

For synchronous iterables, I tried to use "is not iterable" for non-unions, and "may not be iterable" if the inferred type was a union where one element in the union was iterable but one or more other elements were not

---

_@AlexWaygood approved on 2025-07-30 14:51_

Nice. I think it would be good to distinguish between "iterability" and "async iterability" in error messages

---

_@sharkdp reviewed on 2025-07-30 15:30_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/comprehensions/basic.md`:150 on 2025-07-30 15:30_

Done, thanks!

---

_Merged by @sharkdp on 2025-07-30 15:40_

---

_Closed by @sharkdp on 2025-07-30 15:40_

---

_Branch deleted on 2025-07-30 15:40_

---

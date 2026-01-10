```yaml
number: 21521
title: "Semantic tokens: consistently add the `DEFINITION` modifier"
type: pull_request
state: merged
author: lucach
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: semantic_tokens_definitions
created_at: 2025-11-19T11:15:43Z
updated_at: 2025-11-19T11:45:39Z
url: https://github.com/astral-sh/ruff/pull/21521
synced_at: 2026-01-10T16:48:02Z
```

# Semantic tokens: consistently add the `DEFINITION` modifier

---

_Pull request opened by @lucach on 2025-11-19 11:15_

## Summary

When computing semantic tokens, there are a few more statements potentially containing definitions than what is currently covered.

This commit reviews all the statements and completes the list of those requiring a custom visitor to add the `DEFINITION` modifier for the appropriate tokens.

Note: I've not added the `DEFINITION` modifier to the target of an augmented assignment. A new test makes that explicit. This is consistent with Pylance's behavior, although one could argue that given that *all* "regular" assignments are marked as definitions, perhaps also the augmented assignments should.

## Test Plan

Added new tests for each new statement with a custom visitor. When a test was already existing, I've reviewed and applied the diff.

---

_Review requested from @carljm by @lucach on 2025-11-19 11:15_

---

_Review requested from @MichaReiser by @lucach on 2025-11-19 11:15_

---

_Review requested from @AlexWaygood by @lucach on 2025-11-19 11:15_

---

_Review requested from @sharkdp by @lucach on 2025-11-19 11:15_

---

_Review requested from @dcreager by @lucach on 2025-11-19 11:15_

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 11:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-19 11:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-11-19 11:20_

---

_Label `ty` added by @AlexWaygood on 2025-11-19 11:20_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-19 11:20_

---

_@MichaReiser approved on 2025-11-19 11:25_

Thank you

Handling augmented assignments the way you do in this PR seems right to me. 

---

_Comment by @MichaReiser on 2025-11-19 11:26_

Can you run the `ty_server` tests and update the semantic tokens snapshot

---

_Comment by @lucach on 2025-11-19 11:39_

Whoops, sorry. I even ran them, I just overlooked the output 🤦‍♂️

---

_Merged by @MichaReiser on 2025-11-19 11:45_

---

_Closed by @MichaReiser on 2025-11-19 11:45_

---

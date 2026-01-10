```yaml
number: 19848
title: "[ty] Update goto range for attribute access to only target the attribute"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: goto-attribute-range
created_at: 2025-08-10T20:27:36Z
updated_at: 2025-11-06T11:48:16Z
url: https://github.com/astral-sh/ruff/pull/19848
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Update goto range for attribute access to only target the attribute

---

_Pull request opened by @MatthewMckee4 on 2025-08-10 20:27_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

We previously highlighted the whole attribute access for goto definition. This does not align with other language servers (rust-analyzer, python, etc).

More related discussion has been had here https://github.com/astral-sh/ty/issues/169.

But I think for goto definition it seems unintuitive to highlight the whole expression

## Test Plan

Update ty_ide tests


---

_Comment by @github-actions[bot] on 2025-08-10 20:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-10 20:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MatthewMckee4 on 2025-08-10 20:35_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-10 20:35_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-10 20:35_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-10 20:35_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-10 20:35_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-10 20:35_

---

_Renamed from "Update goto range for attribute access to only target the attribute" to "[ty] Update goto range for attribute access to only target the attribute" by @MatthewMckee4 on 2025-08-10 20:37_

---

_Label `server` added by @AlexWaygood on 2025-08-10 20:41_

---

_Label `ty` added by @AlexWaygood on 2025-08-10 20:41_

---

_Comment by @MichaReiser on 2025-08-11 06:39_

Thank you. This makes sense to me. I still think it should be consistent with how we highlight attribute access in diagnostics, where other Python LSPs also only highlight the attribute part (and not the object). Which makes sense to me, but it seems we don't have alignment as a team. 


---

_Comment by @AlexWaygood on 2025-08-11 09:48_

I'm not sure I see a strong argument for consistency here -- to me, the range we show for an invalid attribute-access operation seems like a pretty different concept to the range we use as the target for jumping to the original definition/declaration of an attribute. I don't have a very strong opinion, though.

---

_Comment by @MichaReiser on 2025-08-11 11:43_

I guess that depends on the reasoning why it's preferable to use `self.x` for diagnostics but not for go-to definition. For me, it seems to be similar enough in concept to at least treat it the same (which other type checkers do).

I did a quick check and TypeScript, Pyright, and Rust all highlight the attribute portion only in diagnostics and in goto definition. I still think we should just change our diagnostics. 

Maybe I should stop arguing and just merge this to have another reason why we should change the diagnostic ;) 

---

_@MichaReiser approved on 2025-08-11 14:23_

---

_Comment by @MichaReiser on 2025-08-11 14:23_

I'm okay going forward as it brings me closer to my goal, changing the range on the diagnostic and it matches my expectation for go to definition ;) It's also something that's easy to revert. Thank you

---

_Merged by @MichaReiser on 2025-08-11 14:24_

---

_Closed by @MichaReiser on 2025-08-11 14:24_

---

_Branch deleted on 2025-11-06 11:48_

---

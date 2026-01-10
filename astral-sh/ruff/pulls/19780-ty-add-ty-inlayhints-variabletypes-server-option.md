```yaml
number: 19780
title: "[ty] Add `ty.inlayHints.variableTypes` server option"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - configuration
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/inlay-hint-variable-types
created_at: 2025-08-06T10:50:33Z
updated_at: 2025-08-07T13:46:54Z
url: https://github.com/astral-sh/ruff/pull/19780
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add `ty.inlayHints.variableTypes` server option

---

_Pull request opened by @dhruvmanila on 2025-08-06 10:50_

## Summary

This PR adds a new `ty.inlayHints.variableTypes` server setting to configure ty to include / exclude inlay hints at variable position.

Currently, we only support inlay hints at this position so this option basically translates to enabling / disabling inlay hints for now :)

The VS Code extension PR is https://github.com/astral-sh/ty-vscode/pull/112.

closes: astral-sh/ty#472

## Test Plan

Add E2E tests.


---

_Label `server` added by @dhruvmanila on 2025-08-06 10:50_

---

_Label `ty` added by @dhruvmanila on 2025-08-06 10:50_

---

_Comment by @github-actions[bot] on 2025-08-06 10:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-06 10:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @dhruvmanila on 2025-08-07 11:12_

One thing that I've noticed with other server is that they start off with `ty.inlayHints.variableTypes.enable` option so that any other related option under the "variableTypes" namespace can be accommodated in the future. This PR doesn't do that but we can do it if that's more useful.

---

_Renamed from "[ty] Add `ty.inlayHints.variablesTypes` server option" to "[ty] Add `ty.inlayHints.variableTypes` server option" by @dhruvmanila on 2025-08-07 11:16_

---

_Label `configuration` added by @dhruvmanila on 2025-08-07 11:17_

---

_Marked ready for review by @dhruvmanila on 2025-08-07 11:17_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-07 11:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-08-07 11:17_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-07 11:17_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-07 11:17_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-07 11:17_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-08-07 11:18_

---

_Review request for @carljm removed by @dhruvmanila on 2025-08-07 11:18_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-08-07 11:18_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-08-07 11:18_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:128 on 2025-08-07 11:31_

It seems unnecessary to traverse the entire tree only to bail out at the assignment level. Should we add a `InlayHintSettings::any_enabled` and bail out in the inlay hint request if it returns false to avoid doing all this unnecessary work?

---

_@MichaReiser approved on 2025-08-07 11:31_

---

_@dhruvmanila reviewed on 2025-08-07 12:53_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:128 on 2025-08-07 12:53_

Yeah, that makes sense.

---

_@dhruvmanila reviewed on 2025-08-07 12:56_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:128 on 2025-08-07 12:56_

Actually, I don't think we should add that today because there will be (in the future) multiple positions for which we won't provide a way to disable. I don't think the server should provide configuring every position of inlay hints. So, I'm going to avoid adding this today.

---

_@MichaReiser reviewed on 2025-08-07 13:02_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:128 on 2025-08-07 13:02_

Can you give an example of inlays that the user can't disable and we'd always provide (I'm not disagreeing, just curious)?

---

_@dhruvmanila reviewed on 2025-08-07 13:25_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:128 on 2025-08-07 13:25_

Actually, I don't have an example on top of my mind and it might be that it doesn't exists 😅 

---

_Merged by @dhruvmanila on 2025-08-07 13:46_

---

_Closed by @dhruvmanila on 2025-08-07 13:46_

---

_Branch deleted on 2025-08-07 13:46_

---

```yaml
number: 21001
title: "[ty] Implement go-to for binary and unary operators"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/unary-bin-op-goto
created_at: 2025-10-20T17:40:53Z
updated_at: 2025-10-21T17:30:46Z
url: https://github.com/astral-sh/ruff/pull/21001
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Implement go-to for binary and unary operators

---

_@MichaReiser_

## Summary

This PR implements goto and hover for binary operations and unary expressions. 

As for other hover targets, the implementation doesn't try to match overloads yet. 

This PR doesn't implement go-to support for augmented assign, which seems to match Pylance. I don't see a reason why we couldn't but I suggest leaving that for a separate PR


Fixes https://github.com/astral-sh/ty/issues/1151


## Test plan

Added tests

https://github.com/user-attachments/assets/5b04daea-c04f-4937-a899-2c9879e2d99a



---

_Label `server` added by @MichaReiser on 2025-10-20 17:40_

---

_Label `ty` added by @MichaReiser on 2025-10-20 17:40_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_definition.rs`:801 on 2025-10-21 10:12_

I moved this to the end. It felt weird having it in the middle of the tests

---

_@MichaReiser reviewed on 2025-10-21 10:12_

---

_Comment by @github-actions[bot] on 2025-10-21 10:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:974 on 2025-10-21 10:14_

It didn't feel worth pulling this out into its own `Type` method, especially because `__bool__` is somewhat complicated when all we need here is to get the`__bool__` dunder

---

_@MichaReiser reviewed on 2025-10-21 10:14_

---

_Comment by @github-actions[bot] on 2025-10-21 10:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-10-21 11:26_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call.rs`:29 on 2025-10-21 11:26_

I extracted this from `builder.rs`. The lines are unchanged except that the `map(|outcome| outcome.return_type())` remains in `builder.rs`

---

_Marked ready for review by @MichaReiser on 2025-10-21 11:26_

---

_Review requested from @carljm by @MichaReiser on 2025-10-21 11:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-21 11:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-21 11:26_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-21 11:26_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-10-21 11:26_

---

_Review requested from @Gankra by @MichaReiser on 2025-10-21 11:44_

---

_Review request for @dcreager removed by @MichaReiser on 2025-10-21 11:44_

---

_Review request for @carljm removed by @MichaReiser on 2025-10-21 11:44_

---

_Comment by @github-actions[bot] on 2025-10-21 12:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:939 on 2025-10-21 14:35_

Just out of curiosity, is this to avoid returning `Some` when the cursor is inside a comment?

---

_@dhruvmanila approved on 2025-10-21 15:01_

Looks good, thanks!

---

_@MichaReiser reviewed on 2025-10-21 15:58_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:939 on 2025-10-21 15:58_

Short answer: yes :)

The problem with binary expressions and unary expressions is that we don't know the operator's range. That means, we would return `Some` for

```py
(
	a + # comment<CURSOR>
	b
)
```

Because the range is between `a` and `b`. But that's obviously not what we want. 

---

_Comment by @AlexWaygood on 2025-10-21 16:04_

One _tiny_ UI nit (possibly not solvable, possibly playground-specific, just posting it because I noticed it):

If I have the `cmd` key held down and hover over a symbol, the symbol changes colour and becomes underlined, and my mouse symbol changes to a little ✋ to tell me that I can click on it to go to its definition. But the same thing doesn't happen if I have the `cmd` key held down and hover over an operator, so it's hard for me to tell that cmd-clicking on it is going to take me to the method being displayed by the on-hover tooltip. Here's a video in which I have the `cmd` key held down for the duration:

https://github.com/user-attachments/assets/503ce993-572c-4a66-a91e-87433be73df2

---

_@AlexWaygood reviewed on 2025-10-21 16:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:974 on 2025-10-21 16:13_

You could _possibly_ consider adding methods like these to the AST nodes directly, similar to what we already have for the bin-op nodes:

https://github.com/astral-sh/ruff/blob/e1cada1ec30d289acb00390058fab8bce765836e/crates/ruff_python_ast/src/nodes.rs#L2546-L2602

But yeah, `not` is complicated because it falls back to `__len__` as well as `__bool__`, even if you ignore all the fancy special casing we have baked in for the truthiness of various types.

---

_@AlexWaygood reviewed on 2025-10-21 16:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:1002 on 2025-10-21 16:13_

```suggestion
/// This is so that we show e.g. `int.__add__` instead of `Literal[4].__add__`.
```

---

_@AlexWaygood approved on 2025-10-21 16:14_

Really cool. The core logic looks great!

---

_Comment by @MichaReiser on 2025-10-21 16:27_

> If I have the cmd key held down and hover over a symbol, the symbol changes colour and becomes underlined, and my mouse symbol changes to a little ✋ to tell me that I can click on it to go to its definition. But the same thing doesn't happen if I have the cmd key held down and hover over an operator, so it's hard for me to tell that cmd-clicking on it is going to take me to the method being displayed by the on-hover tooltip. Here's a video in which I have the cmd key held down for the duration:

I noticed this too but it's not clear to me why it is happening, because we report the full range of the binary expression as its range. I can try to use a narrower range to see if that helps

---

_Comment by @MichaReiser on 2025-10-21 17:04_

Nope, that doesn't help (but I think is strictly better). I did give r-a a try and it also doesn't underline `+` and other operands. We also correctly set the [`originSelectionRange`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#locationLink) but it seems VS code doesn't care?

It does work fine with `not`.

Confirmed, Zed renders this correctly

---

_Merged by @MichaReiser on 2025-10-21 17:25_

---

_Closed by @MichaReiser on 2025-10-21 17:25_

---

_Branch deleted on 2025-10-21 17:25_

---

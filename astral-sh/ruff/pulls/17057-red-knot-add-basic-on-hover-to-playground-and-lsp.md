```yaml
number: 17057
title: "[red-knot] Add basic on-hover to playground and LSP"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/hover
created_at: 2025-03-29T16:32:36Z
updated_at: 2025-04-04T13:25:30Z
url: https://github.com/astral-sh/ruff/pull/17057
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add basic on-hover to playground and LSP

---

_@MichaReiser_

## Summary

Implement a very basic hover in the playground and LSP.

It's basic, because it only shows the type on-hover. Most other LSPs also show:

* The signature of the symbol beneath the cursor. E.g. `class Test(a:int, b:int)` (we want something like https://github.com/microsoft/pyright/blob/54f7da25f9c2b6253803602048b04fe0ccb13430/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L21929-L22129)
* The symbols' documentation
* Do more fancy markdown rendering

I decided to defer these features for now because it requires new semantic APIs (similar to *goto definition*), and investing in fancy rendering only makes sense once we have the relevant data.

Closes [#16826](https://github.com/astral-sh/ruff/issues/16826)

## Test Plan


https://github.com/user-attachments/assets/044aeee4-58ad-4d4e-9e26-ac2a712026be

https://github.com/user-attachments/assets/4a1f4004-2982-4cf2-9dfd-cb8b84ff2ecb



---

_Label `server` added by @MichaReiser on 2025-03-29 16:33_

---

_Label `red-knot` added by @MichaReiser on 2025-03-29 16:33_

---

_Comment by @github-actions[bot] on 2025-03-29 16:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[red-knot] LSP and playground hover" to "[red-knot] Add basic on-hover to playground and LSP" by @MichaReiser on 2025-04-03 09:29_

---

_Label `server` removed by @MichaReiser on 2025-04-03 09:29_

---

_Comment by @github-actions[bot] on 2025-04-03 10:01_

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

_Marked ready for review by @MichaReiser on 2025-04-03 10:03_

---

_Review requested from @carljm by @MichaReiser on 2025-04-03 10:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-03 10:03_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-03 10:03_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-03 10:03_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-03 10:03_

---

_Comment by @dhruvmanila on 2025-04-03 21:36_

> * Do more fancy markdown rendering

I'm not sure if we need to do anything here as I think it's the client that will do the rendering, the server just need to make sure to use the [`MarkupKind::Markdown`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#markupContent).

Edit: I think I know what you mean - it's more related to how we generate the markdown content similar to how currently we're using horizontal line to split between signature and (in the future) documentation?

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/hover.rs`:24 on 2025-04-03 21:56_

I like what Pyright is doing here as it's format is `(<node type>) <name>: type` or `(<node type>) <signature>`. I also like what rust-analyzer does which provides a hierarchy of where the symbol is coming from:
```
<import location>

<impl block>
<signature>
---
<documentation>
```
These are just something that I've noticed and there are no action items here except to take inspiration and iterate on the format of the hover content.

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/hover.rs`:68 on 2025-04-03 21:57_

These two looks the same except for only the `IntoIter` type, what's the reason for having these two implementations?

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/hover.rs`:118 on 2025-04-03 21:58_

Should we use "python" as the code block language here?

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/goto.rs`:188 on 2025-04-03 22:05_

Is this change to get the last matched token instead of the first matched token?

---

_@dhruvmanila approved on 2025-04-03 22:06_

Looks pretty straightforward to me!

I haven't looked at the playground code in detail, mainly glanced through it.

---

_Review request for @carljm removed by @carljm on 2025-04-03 23:22_

---

_@MichaReiser reviewed on 2025-04-04 06:08_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/hover.rs`:68 on 2025-04-04 06:08_

The difference is that one allows iterating when you have `self` and the other when you only have a `&self`

---

_@MichaReiser reviewed on 2025-04-04 06:10_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/hover.rs`:118 on 2025-04-04 06:10_

I first used `python` but the notation used by our type rendering isn't valid Python. For example, we have some types of the form `<some text here>`. This resulted in `text here` to be highlighted in red because it isn't valid python. 

I think the long term solution here is to implement a type renderer that more closely matches valid `Python` syntax (or even renders a full signature).

---

_@MichaReiser reviewed on 2025-04-04 06:13_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/goto.rs`:188 on 2025-04-04 06:13_

The change is to get the token with the highest priority and it was necessary to support hovering over the operator in a binary expression. Before, the logic filtered out any non `Name` or literal token, meaning that hovering `+` showed no type. 

The logic now ranks the tokens, preferring names and literals. The "preferring" is important for if you have `call<CURSOR>(`; in this case, we want to get the `call` name and not `(` (or `.<CURSOR>arg`) because it's more likely what the user wanted to hover / goto. 

---

_Merged by @MichaReiser on 2025-04-04 06:13_

---

_Closed by @MichaReiser on 2025-04-04 06:13_

---

_Branch deleted on 2025-04-04 06:13_

---

_@dhruvmanila reviewed on 2025-04-04 13:25_

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/goto.rs`:188 on 2025-04-04 13:25_

> The change is to get the token with the highest priority and it was necessary to support hovering over the operator in a binary expression. Before, the logic filtered out any non `Name` or literal token, meaning that hovering `+` showed no type.

Interesting, so I think this would affect goto implementations as well. For example, in an addition expression like `a + b`, would we go to the type of `a` or `b`? Regardless, it's not a major issue.

---

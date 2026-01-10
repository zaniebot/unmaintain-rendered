```yaml
number: 20466
title: "[ty] Swap `detail` and `description` fields for `CompletionItemLabelDetails`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: ag/fix-weird-type-rendering
created_at: 2025-09-18T12:30:44Z
updated_at: 2025-09-18T13:11:20Z
url: https://github.com/astral-sh/ruff/pull/20466
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Swap `detail` and `description` fields for `CompletionItemLabelDetails`

---

_Pull request opened by @BurntSushi on 2025-09-18 12:30_

This seems to be more consistent with how other LSPs work (like
`rust-analyzer`), and also I think is more consistent with how
`CompletionItem.detail` is itself rendered. Namely, in VS Code, it
is right-aligned. And it's also where we put the type signature.
But `CompletionItemLabelDetails.detail` is left-aligned where as
`CompletionItemLabelDetails.description` is right-aligned. So let's
swap them such that type signatures go in the latter and not the
former.

Fixes https://github.com/astral-sh/ty/issues/1200


---

_Review requested from @carljm by @BurntSushi on 2025-09-18 12:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-18 12:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-18 12:30_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-18 12:30_

---

_Review request for @dcreager removed by @BurntSushi on 2025-09-18 12:31_

---

_Review request for @carljm removed by @BurntSushi on 2025-09-18 12:31_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-09-18 12:31_

---

_Comment by @BurntSushi on 2025-09-18 12:31_

Demo:

https://github.com/user-attachments/assets/f0f1a5bd-332f-4009-ae0b-da98e683331d



---

_Comment by @github-actions[bot] on 2025-09-18 12:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @MichaReiser on 2025-09-18 12:36_

Thanks for looking into this. 

It now seems unfortunate that the module name comes directly (without any space) after the completion text which seems expected acording to the LSP specification

```ts
/**
 * Additional details for a completion item label.
 *
 * @since 3.17.0
 */
export interface [CompletionItemLabelDetails](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemLabelDetails) {

	/**
	 * An optional string which is rendered less prominently directly after
	 * {@link CompletionItem.label label}, without any spacing. Should be
	 * used for function signatures or type annotations.
	 */
	detail?: string;

	/**
	 * An optional string which is rendered less prominently after
	 * {@link CompletionItemLabelDetails.detail}. Should be used for fully qualified
	 * names or file path.
	 */
	description?: string;
}
```

I think we should at least insert a space at the front of `detail` if we want to use that field.

Or we keep using `detail` for the type but change the syntax to `: {type}`, similar to what we show in inlay hints. Wdyt? 

Edit: I only just now saw your detailed write-up on the issue. I think what you did makes sense. I'm only wondering if we should introduce a space to make this look less squeezed together

Edit2: It seems that r-a solves this visual issue by using `(use <module name>)` for the left aligned content which helps a bit.

---

_Comment by @github-actions[bot] on 2025-09-18 12:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @BurntSushi on 2025-09-18 12:49_

Yeah and r-a inserts a leading space too.

I added the leading space and added `(import <name>)`, which I think also helps (similar r-a's `(use <name>)`).

Demo:


https://github.com/user-attachments/assets/6e6a4019-6552-4b7a-b787-416695b2692f



---

_Label `bug` added by @MichaReiser on 2025-09-18 12:53_

---

_Label `server` added by @MichaReiser on 2025-09-18 12:53_

---

_@MichaReiser approved on 2025-09-18 12:53_

---

_@sharkdp approved on 2025-09-18 13:11_

Thank you!

---

_Merged by @BurntSushi on 2025-09-18 13:11_

---

_Closed by @BurntSushi on 2025-09-18 13:11_

---

_Branch deleted on 2025-09-18 13:11_

---

```yaml
number: 1200
title: Types are weirdly attached to symbols in auto-completions
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-09-18T07:26:49Z
updated_at: 2025-10-03T12:19:10Z
url: https://github.com/astral-sh/ty/issues/1200
synced_at: 2026-01-12T15:54:24Z
```

# Types are weirdly attached to symbols in auto-completions

---

_@sharkdp_

I don't know when this changed, but this looks weird:

<img width="545" height="315" alt="Image" src="https://github.com/user-attachments/assets/c291c6d5-44b3-4cef-946f-02c493fb8559" />

Maybe just a problem with my VS code setup, but I don't remember changing anything?

---

_Label `server` added by @sharkdp on 2025-09-18 07:26_

---

_Comment by @MichaReiser on 2025-09-18 07:37_

@BurntSushi can you take a look at this as it might be related to your most recent changes to `Completion`

---

_Label `bug` added by @MichaReiser on 2025-09-18 07:38_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-09-18 11:59_

---

_Comment by @BurntSushi on 2025-09-18 12:31_

OK, so I think I understand what is happening here. Prior to https://github.com/astral-sh/ruff/pull/20439, I was _only_ setting [`CompletionItem.detail`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItem). But after that PR, I am _also_ setting [`CompletionItem.labelDetails`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItem) to a [`CompletionItemLabelDetails`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemLabelDetails).

To set them, I followed the recommendation in the docs. That is, `CompletionItemLabelDetails.detail` has an example of a type signature going into it while `CompletionItemLabelDetails.description` includes a fully qualified name as an example. Indeed, that's what I did:

https://github.com/astral-sh/ruff/blob/48ada2d359b7c985454f4ac7ae7f23d1e980865a/crates/ty_server/src/server/api/requests/completion.rs#L88-L92

Notice that `CompletionItem.detail` and `CompletionItemLabelDetails.detail` are identical. I think that's because not all LSP clients support the latter, so there is some redundancy in the LSP data types. Moreover, the docs for these two fields also exemplify redundancy:

```

	/**
	 * A human-readable string with additional information
	 * about this item, like type or symbol information.
	 */
	detail?: string;
```

and

```
export interface CompletionItemLabelDetails {
	/**
	 * An optional string which is rendered less prominently directly after
	 * {@link CompletionItem.label label}, without any spacing. Should be
	 * used for function signatures or type annotations.
	 */
	detail?: string;
```

So I think what I have now is at least consistent with the LSP protocol docs.

... but, it seems like this may not be common practice? If I go look at what `rust-analyzer` does, it calls `CompletionItemLabelDetails.detail` "label left" and `CompletionItemLabelDetails.description` "label right":

https://github.com/rust-lang/rust-analyzer/blob/e10fa9393ea2df4067e2258c9b8132244e415964/crates/rust-analyzer/src/lsp/to_proto.rs#L386-L399

Those names come from how `detail` and `description` are _rendered_. `detail` gets left aligned (without spacing between it and the symbol name) where as `description` gets right aligned. Notably, the top-level `CompletionItem.detail` gets right aligned too it seems, at least in practice (although this isn't part of the docs, unlike for the fields in `CompletionItemLabelDetails`).

And indeed, `rust-analyzer` puts the type signature in the _right_ aligned field, that is, `CompletionItemLabelDetails.description`:

https://github.com/rust-lang/rust-analyzer/blob/e10fa9393ea2df4067e2258c9b8132244e415964/crates/ide-completion/src/item.rs#L547-L582

(It also uses the left aligned field for various things, depending on what the completion is, including the module path if it's a symbol that isn't already in scope.)

And indeed, it puts the type signature in the _right_ aligned spot.

Anywho, all we need to do is swap the fields: https://github.com/astral-sh/ruff/pull/20466

---

_Closed by @BurntSushi on 2025-09-18 13:11_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:19_

---

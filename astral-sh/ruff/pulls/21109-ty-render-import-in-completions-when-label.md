```yaml
number: 21109
title: "[ty] Render `import <...>` in completions when \"label details\" isn't supported"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/fix-label-details
created_at: 2025-10-28T15:34:37Z
updated_at: 2025-10-29T12:50:43Z
url: https://github.com/astral-sh/ruff/pull/21109
synced_at: 2026-01-12T15:57:16Z
```

# [ty] Render `import <...>` in completions when "label details" isn't supported

---

_@BurntSushi_

This fixes a bug where the `import module` part of a completion for
unimported candidates would be missing. This makes it especially
confusing because the user can't tell where the symbol is coming from,
and there is no hint that an `import` statement will be inserted.

Previously, we were using [`CompletionItemLabelDetails`] to render the
`import module` part of the suggestion. But this is only supported in
clients that support version 3.17 (or newer) of the LSP specification.
It turns out that this support isn't widespread yet. In particular,
Heliex doesn't seem to support "label details."

To fix this, we take a [cue from rust-analyzer][rust-analyzer-details].
We detect if the client supports "label details," and if so, use it.
Otherwise, we push the `import module` text into the completion label
itself.

Fixes https://github.com/astral-sh/ruff/pull/20439#issuecomment-3313689568

[`CompletionItemLabelDetails`]: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemLabelDetails
[rust-analyzer-details]: https://github.com/rust-lang/rust-analyzer/blob/5d905576d49233ed843bb40df4ed57e5534558f5/crates/rust-analyzer/src/lsp/to_proto.rs#L391-L404


---

_Review requested from @carljm by @BurntSushi on 2025-10-28 15:34_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-10-28 15:34_

---

_Review requested from @sharkdp by @BurntSushi on 2025-10-28 15:34_

---

_Review requested from @dcreager by @BurntSushi on 2025-10-28 15:34_

---

_Comment by @BurntSushi on 2025-10-28 15:35_

Demo:

https://github.com/user-attachments/assets/39ccbdb4-edfd-416b-b3f9-14d95e5fbe82

In this demo, I show the VS Code rendering, which is unchanged. Then I show Helix. Previously, it didn't include the modules for unimported symbols. But now it does!

---

_Review request for @dcreager removed by @BurntSushi on 2025-10-28 15:35_

---

_Review request for @carljm removed by @BurntSushi on 2025-10-28 15:35_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-10-28 15:35_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-10-28 15:35_

---

_Comment by @github-actions[bot] on 2025-10-28 15:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-28 15:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-10-28 17:41_

---

_Label `ty` added by @MichaReiser on 2025-10-28 17:41_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:98 on 2025-10-28 17:44_

Is there a leading whitespace at the start of `import_suffix` (I'm asking because it's hard to tell from Github's review page and I can't load your test plan with the onboard wifi ;)).

If not, should we add one?

---

_@MichaReiser approved on 2025-10-28 17:44_

---

_@BurntSushi reviewed on 2025-10-29 12:50_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:98 on 2025-10-29 12:50_

Yes there is! From above:

```rust
let import_suffix = comp.module_name.map(|name| format!(" (import {name})"));
```

---

_Merged by @BurntSushi on 2025-10-29 12:50_

---

_Closed by @BurntSushi on 2025-10-29 12:50_

---

_Branch deleted on 2025-10-29 12:50_

---

```yaml
number: 18917
title: "[ty] Enforce sort order of completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-vs-code-sort
created_at: 2025-06-24T14:29:59Z
updated_at: 2025-06-24T15:31:10Z
url: https://github.com/astral-sh/ruff/pull/18917
synced_at: 2026-01-12T15:56:27Z
```

# [ty] Enforce sort order of completions

---

_@BurntSushi_

We achieve this by setting the "sort text" field of every completion.
Since we are trying to be smart about the order, we want the client to
respect our order.

Prior to this change, VS Code was re-sorting completions in
lexicographic order. This in turn resulted in dunder attributes
appearing before "normal" attributes.


---

_Review requested from @carljm by @BurntSushi on 2025-06-24 14:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-24 14:30_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-24 14:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-24 14:30_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-24 14:30_

---

_Comment by @BurntSushi on 2025-06-24 14:30_

Demo:

https://github.com/user-attachments/assets/1441352d-dcf6-4d2c-89cd-02aa24ec9124



---

_Comment by @BurntSushi on 2025-06-24 14:30_

I wonder if this means we can revert https://github.com/astral-sh/ruff/pull/18480? cc @MichaReiser 

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-24 14:30_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-24 14:30_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-06-24 14:30_

---

_Comment by @github-actions[bot] on 2025-06-24 14:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-06-24 14:46_

> I wonder if this means we can revert #18480? cc @MichaReiser

I don't think we can because this and #18480 do the same at the API boundary (but they're different boundaries). We could add the `sortText` to the `CompletionItem` that we return internally so that we don't need to repeat the implementation twice, but I don't think that's worth it.

rust analyzer does the same, but in a little fancier way:

https://github.com/rust-lang/rust-analyzer/blob/59ef84506deb45e6d4336935d0d4be54c3386814/crates/rust-analyzer/src/lsp/to_proto.rs#L455-L471

---

_Label `server` added by @MichaReiser on 2025-06-24 14:46_

---

_Label `ty` added by @MichaReiser on 2025-06-24 14:46_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:54 on 2025-06-24 14:47_

```suggestion
        let max_index_len = completions.len().saturating_sub(1);
```

It's not the max_index but it's length

---

_@MichaReiser approved on 2025-06-24 14:47_

---

_@BurntSushi reviewed on 2025-06-24 15:14_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:54 on 2025-06-24 15:14_

Right. And I forgot the `.to_string().len()` here too!

---

_Comment by @AlexWaygood on 2025-06-24 15:24_

you probably want reviews from Micha and/or Dhruv when it comes to the bits of autocompletion that involve actually talking to the LSP client -- the only LSP I know anything about is the Liskov Substitution Principle 😛

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-24 15:24_

---

_@MichaReiser reviewed on 2025-06-24 15:26_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:54 on 2025-06-24 15:26_

I mean, padding all the way to `max_index` works too ;)

---

_Merged by @BurntSushi on 2025-06-24 15:31_

---

_Closed by @BurntSushi on 2025-06-24 15:31_

---

_Branch deleted on 2025-06-24 15:31_

---

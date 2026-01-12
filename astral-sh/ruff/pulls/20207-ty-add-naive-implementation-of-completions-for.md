```yaml
number: 20207
title: "[ty] Add naive implementation of completions for unimported symbols"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/all-symbols
created_at: 2025-09-02T15:00:51Z
updated_at: 2025-09-03T13:57:28Z
url: https://github.com/astral-sh/ruff/pull/20207
synced_at: 2026-01-12T15:56:56Z
```

# [ty] Add naive implementation of completions for unimported symbols

---

_@BurntSushi_

This PR takes a first step toward implementing auto-import (see #535).

There are two main reasons why this is considered naive:

1. Once a helping of dependencies have been added to an environment,
   completions can become quite slow. Into the hundreds of milliseconds.
   I've intentionally not spent any time on performance here. We're
   just using a per-file Salsa caching mechanism and searching is done
   exhaustively.
2. When a completion for an unimported symbol is selected, the necessary
   import is not yet added. (This is what I plan to focus on next.)

For those two reasons, these completions will only appear when users opt
into them via the `ty.experimental.autoImport` setting.

This PR is best reviewed commit by commit.


---

_Review requested from @carljm by @BurntSushi on 2025-09-02 15:00_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-09-02 15:00_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-02 15:00_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-02 15:00_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-02 15:00_

---

_Review request for @dcreager removed by @BurntSushi on 2025-09-02 15:01_

---

_Review request for @carljm removed by @BurntSushi on 2025-09-02 15:01_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-09-02 15:01_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-09-02 15:01_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-09-02 15:01_

---

_Comment by @github-actions[bot] on 2025-09-02 15:03_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @BurntSushi on 2025-09-02 15:03_

Demo:

https://github.com/user-attachments/assets/f960feb7-3cf0-4089-b7f8-7bceb3fda3e9



---

_Comment by @github-actions[bot] on 2025-09-02 15:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-02 15:28_

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

_Comment by @AlexWaygood on 2025-09-02 15:33_

> 2\. When a completion for an unimported symbol is selected, the necessary
>         import is not yet added. (This is what I plan to focus on next.)

We already have quite good infrastructure for this for some Ruff autofixes. It might be worth seeing if that can be factored out and put somewhere where it could be reused by ty. (It also might be completely impossible to reuse it, but it could be worth a look.) It's found here: https://github.com/astral-sh/ruff/tree/main/crates/ruff_linter/src/importer

---

_Comment by @BurntSushi on 2025-09-02 15:36_

@AlexWaygood Nice yeah! I was planning to look for that. Thanks for finding it. :-)

---

_Label `server` added by @AlexWaygood on 2025-09-02 22:01_

---

_Label `ty` added by @AlexWaygood on 2025-09-02 22:01_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/all_symbols.rs`:67 on 2025-09-03 08:25_

I think it's fine but I was wondering if we want to restrict the test cases to avoid the builtins because any changes to typeshed would require updating the snapshots here if it's related. And, I think this also allows us to get coverage over the builtins symbols which seems useful.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:157 on 2025-09-03 09:15_

Oh shoot, the `unwrap_or` here was meant to be `false`. Can you make this change? Luckily, no one should be affected because there was only one setting in the experimental group.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/module_resolver/module.rs`:1 on 2025-09-03 09:19_

It might be useful to have @AlexWaygood to take a look at the changes here once he is back.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/symbols.rs`:304 on 2025-09-03 09:21_

Yeah, I think it's fine to not do this now. Pylance does provide the icon and the documentation as well.

---

_@dhruvmanila approved on 2025-09-03 09:25_

This looks great!

If it's not already planned, it would be great if you could update the ty documentation for editor settings to include the new setting over at https://github.com/astral-sh/ty/blob/59bf06d2675cc6add7b4ba6f603f70edebcf9499/docs/reference/editor-settings.md#L4 and update the VS Code extension to include the setting ([reference change](https://github.com/astral-sh/ty-vscode/commit/e325761ea4f547b03fec7d9d26c9c7dae8851321)).

---

_@BurntSushi reviewed on 2025-09-03 13:31_

---

_Review comment by @BurntSushi on `crates/ty_server/src/session/options.rs`:157 on 2025-09-03 13:31_

Yup, done!

---

_@BurntSushi reviewed on 2025-09-03 13:32_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/module.rs`:1 on 2025-09-03 13:32_

Aye. I'd love that. I'll probably merge before then, but I'd be happy to address feedback post-merge.

(I think one known deficiency is that namespace packages aren't really supported. Which matches the status quo. I think they're going to need a dedicated strategy to deal with.)

---

_@BurntSushi reviewed on 2025-09-03 13:42_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:67 on 2025-09-03 13:42_

Seems sensible. Done!

---

_Merged by @BurntSushi on 2025-09-03 13:57_

---

_Closed by @BurntSushi on 2025-09-03 13:57_

---

_Branch deleted on 2025-09-03 13:57_

---

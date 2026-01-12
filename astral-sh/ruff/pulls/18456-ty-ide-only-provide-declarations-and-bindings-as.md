```yaml
number: 18456
title: "[ty] IDE: only provide declarations and bindings as completions"
type: pull_request
state: merged
author: sharkdp
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: david/ide-only-suggest-declarations-and-bindings
created_at: 2025-06-04T09:14:53Z
updated_at: 2025-06-04T14:11:07Z
url: https://github.com/astral-sh/ruff/pull/18456
synced_at: 2026-01-12T15:56:19Z
```

# [ty] IDE: only provide declarations and bindings as completions

---

_@sharkdp_

## Summary

Previously, all symbols where provided as possible completions. In an example like the following, both `foo` and `f` were suggested as completions, because `f` itself is a symbol.
```py
foo = 1

f<CURSOR>
```
Similarly, in the following example, `hidden_symbol` was suggested, even though it is not statically visible:
```py
if 1 + 2 != 3:
    hidden_symbol = 1

hidden_<CURSOR>
```

With the change suggested here, we only use statically visible declarations and bindings as a source for completions. In the scenario from [this comment](https://github.com/astral-sh/ruff/pull/18425#issuecomment-2935753258), we do not include `ReadableBuffer` anymore (which is available as a symbol, but not re-exported from `typing`). A test for this was also added here.

![image](https://github.com/user-attachments/assets/01cfc206-d036-4828-826b-c2b9dc8202e9)


## Test Plan

- Updated snapshot tests
- New test for statically hidden definitions
- Added test for star import

---

_Label `server` added by @sharkdp on 2025-06-04 09:14_

---

_Label `ty` added by @sharkdp on 2025-06-04 09:14_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:927 on 2025-06-04 09:17_

`float` will be available again once we add all builtins as a source for completions.

---

_@sharkdp reviewed on 2025-06-04 09:17_

---

_Comment by @github-actions[bot] on 2025-06-04 09:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-06-04 09:18_

---

_Review requested from @carljm by @sharkdp on 2025-06-04 09:18_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-04 09:18_

---

_Review requested from @dcreager by @sharkdp on 2025-06-04 09:18_

---

_Review requested from @MichaReiser by @sharkdp on 2025-06-04 09:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:154 on 2025-06-04 09:36_

```suggestion
        self.members.extend(all_declarations_and_bindings(db, scope_id));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:74 on 2025-06-04 09:37_

```suggestion
            symbols.extend(all_declarations_and_bindings(self.db, file_scope.to_scope_id(self.db, self.file)));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:74 on 2025-06-04 09:38_

I still think we might want to include unreachable bindings if the cursor is actually in an unreachable area of code -- it might be worth a TODO? But the autocomplete behaviour for `*` imports is very unintuitive right now where it's suggesting all these symbols that just don't exist at runtime, so this is definitely the right thing to do for now IMO

---

_@AlexWaygood approved on 2025-06-04 09:38_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-04 09:40_

---

_@sharkdp reviewed on 2025-06-04 09:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_model.rs`:74 on 2025-06-04 09:46_

> it might be worth a TODO?

Done, added a test for this.

---

_@AlexWaygood reviewed on 2025-06-04 09:49_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:848 on 2025-06-04 09:49_

```suggestion
        // currently make no effort to provide a good IDE experience within sections that
```

---

_@BurntSushi approved on 2025-06-04 14:07_

Nice!

---

_Merged by @sharkdp on 2025-06-04 14:11_

---

_Closed by @sharkdp on 2025-06-04 14:11_

---

_Branch deleted on 2025-06-04 14:11_

---

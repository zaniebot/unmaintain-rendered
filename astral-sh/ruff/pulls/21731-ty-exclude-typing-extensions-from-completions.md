```yaml
number: 21731
title: "[ty] Exclude `typing_extensions` from completions unless it's really available"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/auto-import-partial-confusion
created_at: 2025-12-01T14:45:44Z
updated_at: 2025-12-01T16:24:19Z
url: https://github.com/astral-sh/ruff/pull/21731
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Exclude `typing_extensions` from completions unless it's really available

---

_@BurntSushi_

This works by adding a third module resolution mode that lets the caller
opt into _some_ shadowing of modules that is otherwise not allowed (for
`typing` and `typing_extensions`).

Fixes astral-sh/ty#1658


---

_Review requested from @carljm by @BurntSushi on 2025-12-01 14:45_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-01 14:45_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-01 14:45_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-01 14:45_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-01 14:45_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-01 14:46_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-01 14:46_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-12-01 14:46_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-01 14:46_

---

_Review requested from @Gankra by @BurntSushi on 2025-12-01 14:46_

---

_Comment by @BurntSushi on 2025-12-01 14:46_

Demo:

https://github.com/user-attachments/assets/a36545ce-b71b-42f7-9f42-df2535e00c94



---

_Label `server` added by @AlexWaygood on 2025-12-01 14:46_

---

_Label `ty` added by @AlexWaygood on 2025-12-01 14:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 14:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 14:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5814 diagnostics
+ Found 5815 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-12-01 15:00_

Ideally I guess we'd also include some tests where we mock out a virtual environment that _does_ include `typing_extensions`, and then check that the `typing_extensions` suggestions are still included. But that sounds a bit involved 🙃

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/all_symbols.rs`:43 on 2025-12-01 15:02_

I think we should still include `typing_extensions` suggestions if it's a stub file, even if `typing_extensions` isn't installed into the environment. Stub files are never executed at runtime, so the typeshed version of `typing_extensions` is fine for them.

Ideally we would also include `typing_extensions` suggestions inside `if TYPE_CHECKING` blocks (which are also not executed at runtime), but that won't produce great results until/unless we implement https://github.com/astral-sh/ty/issues/1553, so I wouldn't worry about it too much right now.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:53 on 2025-12-01 15:03_

hmm, `types` _is_ actually a stdlib module, so it's a bit different from `typing_extensions`, which is a "fake typeshed stdlib module"

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:91 on 2025-12-01 15:04_

same here, I'd remove the mention of `types`; it's not really relevant. That one really _is_ always available at runtime!

---

_@AlexWaygood approved on 2025-12-01 15:05_

---

_@BurntSushi reviewed on 2025-12-01 15:15_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:53 on 2025-12-01 15:15_

Hmmm, I was mostly just leaving what was there before. Or should `types` _always_ be considered non-shadowable?

---

_@AlexWaygood reviewed on 2025-12-01 15:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:53 on 2025-12-01 15:18_

I think `types` _probably_ should _always_ be considered non-shadowable, yes, because it is imported as part of the Python interpreter's startup process so it's very difficult to shadow it in first-party code (even though theoretically possible).

But the main thing I was actually objecting to was the comment! You say "we don't want to pretend as if these modules are always available at runtime" -- but there's no pretence with the `types` module. It just _is_ always available at runtime!

---

_@Gankra reviewed on 2025-12-01 15:19_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:53 on 2025-12-01 15:19_

I believe all actual stdlib modules (which includes `types`) are "always non-shadowable", yeah.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:53 on 2025-12-01 15:25_

> I believe all actual stdlib modules (which includes `types`) are "always non-shadowable", yeah.

no, that's not true -- most stdlib modules can be shadowed by user code. The first-party search paths have precedence over the stdlib search path (emulating the fact that `.` is the first entry on `sys.path` at runtime).

But certain "builtin" modules have special-casing baked deep into the Python interpreter, and these modules skip normal module resolution. These "builtin" modules, a subset of the Python stdlib, must _always_ be resolved to locations in the standard library. Ty emulates this, because it accurately reflects what happens at runtime, and also adds a few other modules which aren't _actually_ builtin at runtime but where we're very likely to crash if the module is resolved to a non-stdlib location (`types`, `typing_extensions`)

---

_@AlexWaygood reviewed on 2025-12-01 15:25_

---

_@BurntSushi reviewed on 2025-12-01 15:40_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:53 on 2025-12-01 15:40_

Got it. Comments fixed and I made `types` always non-shadowable. So only `typing_extensions` is the special snowflake here.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:43 on 2025-12-01 15:40_

Ah good call. That makes sense. Fixed.

---

_@BurntSushi reviewed on 2025-12-01 15:40_

---

_@BurntSushi reviewed on 2025-12-01 15:41_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1 on 2025-12-01 15:41_

Yeah we do at least have tests here with a `typing_extensions.py` in a non-std search path. So I think that covers the most critical case (which was failing before adding the shadowable configuration).

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/all_symbols.rs`:23 on 2025-12-01 15:49_

```suggestion
    // TODO: also make it available in `TYPE_CHECKING` blocks
    // (we'd need https://github.com/astral-sh/ty/issues/1553 to do this well)
    let is_typing_extensions_available = importing_from.is_stub(db)
        || resolve_real_shadowable_module(db, &typing_extensions).is_some();
```

---

_@AlexWaygood reviewed on 2025-12-01 15:49_

---

_Merged by @BurntSushi on 2025-12-01 16:24_

---

_Closed by @BurntSushi on 2025-12-01 16:24_

---

_Branch deleted on 2025-12-01 16:24_

---

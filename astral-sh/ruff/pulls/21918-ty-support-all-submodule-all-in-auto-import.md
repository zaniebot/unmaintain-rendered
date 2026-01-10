```yaml
number: 21918
title: "[ty] Support `__all__ += submodule.__all__` in auto-import"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/support-all-submodule-extend
created_at: 2025-12-11T15:10:28Z
updated_at: 2025-12-12T15:11:07Z
url: https://github.com/astral-sh/ruff/pull/21918
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Support `__all__ += submodule.__all__` in auto-import

---

_Pull request opened by @BurntSushi on 2025-12-11 15:10_

... and also `__all__.extend(submodule.__all__)`.

I originally left out support for this since I was unclear on whether
we'd really need it. But it turns out this is used somewhat frequently.
For example, in `numpy`.

Here, we implement this by cheaply keeping track of imports during
the auto-import AST scan. Then when we see something like
`__all__ += foo.__all__`, we look up `foo` in our tracked imports,
attempt to resolve the module and update `__all__` as appropriate.

This doesn't handle every case (for example, it doesn't account for
redefinitions of module names), but it's hopefully enough for most
cases. The docs on `Imports` and tests show some known cases where
this approach falls short.

I suggest reviewers go commit-by-commit.


---

_Review requested from @carljm by @BurntSushi on 2025-12-11 15:10_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-11 15:10_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-11 15:10_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-11 15:10_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-11 15:10_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-11 15:10_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-11 15:10_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-11 15:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `server` added by @AlexWaygood on 2025-12-11 15:12_

---

_Label `ty` added by @AlexWaygood on 2025-12-11 15:12_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:474 on 2025-12-12 12:41_

```suggestion
    module_names: FxHashMap<&'db str, ImportModuleKind<'db>>,
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:546 on 2025-12-12 12:48_

I think you now encounter a situation where your query potentially becomes cyclic if you have `a.py` importing `b.py` and `b.py` importing `a.py` and they both extend their `__all__` with the result from the other module. 

We should add a test for this. I suspect that Salsa will panic because it detects the cycle and your query doesn't have any cycle handling (which we'd have to add). 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:2234 on 2025-12-12 12:49_

Remove?

---

_@MichaReiser reviewed on 2025-12-12 12:51_

This seems a reasonable heuristic to me. 

The only thing I think is necessary to add (and handle) before landing is an example where the `__all__` definition of two modules is cyclic. I suspect that your implementation will panic if this is the case but Salsa's fixpoint handling has us covered (but may require sorting the symbols in `FlatSymbols` to guarantee convergences)

---

_@BurntSushi reviewed on 2025-12-12 13:01_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:474 on 2025-12-12 13:01_

Any particular reason for this change? I went with `Name` because that's what I'm already using everywhere. (`&str` does work here too...)

---

_@MichaReiser reviewed on 2025-12-12 13:11_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:474 on 2025-12-12 13:11_

I consider `Name` a `String` equivalent that avoids heap allocations. That's why I often pass `&str` if I don't need an owned instance (which also avoids repeating the variant check whenever the string content is accessed)

But, obviously, very nit

---

_@BurntSushi reviewed on 2025-12-12 13:15_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:474 on 2025-12-12 13:15_

Ah I see. OK, that makes sense to me.

---

_@BurntSushi reviewed on 2025-12-12 14:05_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:546 on 2025-12-12 14:05_

OK, this should be fixed now. The fix isn't ideal from a perf perspective, but I'll tackle that later when I look at auto-import perf.

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-12 14:22_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:708 on 2025-12-12 15:08_

Hmm, annoying. Seems tricky to work around

---

_@MichaReiser approved on 2025-12-12 15:08_

---

_@BurntSushi reviewed on 2025-12-12 15:10_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:708 on 2025-12-12 15:10_

Yeah I've got ideas, but requires more refactoring.

---

_Merged by @BurntSushi on 2025-12-12 15:11_

---

_Closed by @BurntSushi on 2025-12-12 15:11_

---

_Branch deleted on 2025-12-12 15:11_

---

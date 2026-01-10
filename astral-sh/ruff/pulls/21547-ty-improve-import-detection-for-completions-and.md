```yaml
number: 21547
title: "[ty] Improve import detection for completions and support `from ...<CURSOR>` completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/better-import-detection
created_at: 2025-11-20T20:14:52Z
updated_at: 2025-11-21T13:01:04Z
url: https://github.com/astral-sh/ruff/pull/21547
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Improve import detection for completions and support `from ...<CURSOR>` completions

---

_Pull request opened by @BurntSushi on 2025-11-20 20:14_

This PR has three main changes in it:

* A small refactor of how we collect completions. This centralizes
some logic and made other refactoring easier.
* All of the old piecemeal import detection heuristics have been
replaced with a giant token-based state machine that is intended to
detect all kinds of (possibly incomplete) import statements.
* We build on this new infrastructure to support module completions
for relative imports.

This is best reviewed commit-by-commit.


---

_Review requested from @carljm by @BurntSushi on 2025-11-20 20:14_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-11-20 20:14_

---

_Review requested from @sharkdp by @BurntSushi on 2025-11-20 20:14_

---

_Review requested from @dcreager by @BurntSushi on 2025-11-20 20:14_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-11-20 20:14_

---

_Comment by @BurntSushi on 2025-11-20 20:15_

Demo:

https://github.com/user-attachments/assets/13c06b9a-bf99-43d9-969d-2ed745cfdb15



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 20:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `server` added by @AlexWaygood on 2025-11-20 20:17_

---

_Label `ty` added by @AlexWaygood on 2025-11-20 20:17_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 20:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:757 on 2025-11-20 20:57_

```suggestion
            /// In the [grammar], this is either `dotted_as_names`
            /// or `import_from_as_names`.
            ///
            /// [grammar]: https://docs.python.org/3/reference/grammar.html
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:823 on 2025-11-20 20:59_

```suggestion
            /// A left parenthesis was just seen.
```

---

_@AlexWaygood reviewed on 2025-11-20 21:15_

The "backwards munching through the tokens" state machine scares me a bit 😆 but this is awesome, thank you

---

_Comment by @AlexWaygood on 2025-11-20 21:16_

One edge case I spotted while playing around: here, I'd expect module names such as `builtins` to be suggested, but auto-import suggestions are given instead: 
<img width="1812" height="806" alt="image" src="https://github.com/user-attachments/assets/a25a01d4-d86c-4813-9758-89a784afa321" />


---

_@AlexWaygood approved on 2025-11-20 21:19_

Thank you!!

---

_Comment by @BurntSushi on 2025-11-20 21:56_

> The "backwards munching through the tokens" state machine scares me a bit 😆 but this is awesome, thank you

Yeah reverse state machines tend to be more complex in my experience. I think it's because we (humans) usually design grammars thinking about parsing them in the forward direction. But the reverse usually requires more states. (This is true in `regex` too, which compiles a forward _and_ reverse state machine for regex patterns. The reverse state machine is almost always bigger and more complicated.)

---

_Comment by @BurntSushi on 2025-11-20 21:57_

> One edge case I spotted while playing around: here, I'd expect module names such as `builtins` to be suggested, but auto-import suggestions are given instead:

Nice find! A bug in the state machine. :-) Should be fixed now.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:60 on 2025-11-20 22:16_

I would do the opposite here and rename add to force_add or similar and rename try_add to just add because

* try_add is what should be used in most places. If in doubt, use try_add. However, try_add is harder to discover, I'm more likely to call add
* it feels inconsistent that extend uses try_add. 

---

_Review request for @dcreager removed by @BurntSushi on 2025-11-20 22:17_

---

_Review request for @carljm removed by @BurntSushi on 2025-11-20 22:17_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1008 on 2025-11-20 22:22_

You could consider using TextRange::new(start, end) as slice argument instead of calling to_usize on both start and end 

---

_@MichaReiser approved on 2025-11-20 22:23_

---

_@BurntSushi reviewed on 2025-11-20 22:33_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:60 on 2025-11-20 22:33_

I agree that `extend` using `try_add` is perhaps not great. Maybe it would be better to add a concrete `try_extend` and remove the `Extend` impl.

I do like keeping `try_add` though, because I think using `add` could be too misleading since it might not actually add the completion.

With that said, I'd be okay with renaming `add` to `force_add`. So we'd have `try_add` and `force_add`.

---

_Merged by @BurntSushi on 2025-11-21 13:01_

---

_Closed by @BurntSushi on 2025-11-21 13:01_

---

_Branch deleted on 2025-11-21 13:01_

---

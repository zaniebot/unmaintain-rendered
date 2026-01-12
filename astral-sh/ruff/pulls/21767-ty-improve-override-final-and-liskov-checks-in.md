```yaml
number: 21767
title: "[ty] Improve `@override`, `@final` and Liskov checks in cases where there are multiple reachable definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/first-declaration
created_at: 2025-12-02T23:50:45Z
updated_at: 2025-12-03T13:56:30Z
url: https://github.com/astral-sh/ruff/pull/21767
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Improve `@override`, `@final` and Liskov checks in cases where there are multiple reachable definitions

---

_@AlexWaygood_

## Summary

(Stacked on top of https://github.com/astral-sh/ruff/pull/21756; review that first.)

Fixes https://github.com/astral-sh/ty/issues/1677. Currently we only keeping track of a `Definition` if there was only one single reachable definition for a `Place`. Now, we always keep track of the first `Definition`, even if there are multiple reachable definitions for that `Place`.

## Test Plan

mdtests


---

_Label `ty` added by @AlexWaygood on 2025-12-02 23:50_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 23:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 23:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain.py:481:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/porcelain.py:485:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 228 diagnostics
+ Found 226 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/_frozen_importlib.pyi:79:13: error[invalid-method-override] Invalid override of method `create_module`: Definition is incompatible with `Loader.create_module`
+ mypy/typeshed/stdlib/_frozen_importlib.pyi:118:13: error[invalid-method-override] Invalid override of method `create_module`: Definition is incompatible with `Loader.create_module`
+ mypy/typeshed/stdlib/array.pyi:65:13: error[invalid-method-override] Invalid override of method `index`: Definition is incompatible with `Sequence.index`
+ mypy/typeshed/stdlib/asyncio/base_events.pyi:325:19: error[invalid-method-override] Invalid override of method `create_server`: Definition is incompatible with `AbstractEventLoop.create_server`
+ mypy/typeshed/stdlib/asyncio/base_events.pyi:325:19: error[invalid-method-override] Invalid override of method `create_server`: Definition is incompatible with `AbstractEventLoop.create_server`
+ mypy/typeshed/stdlib/fractions.pyi:125:13: error[invalid-method-override] Invalid override of method `__pow__`: Definition is incompatible with `Complex.__pow__`
+ mypy/typeshed/stdlib/fractions.pyi:125:13: error[invalid-method-override] Invalid override of method `__pow__`: Definition is incompatible with `Complex.__pow__`
+ mypy/typeshed/stdlib/fractions.pyi:135:13: error[invalid-method-override] Invalid override of method `__rpow__`: Definition is incompatible with `Complex.__rpow__`
+ mypy/typeshed/stdlib/fractions.pyi:135:13: error[invalid-method-override] Invalid override of method `__rpow__`: Definition is incompatible with `Complex.__rpow__`
- mypy/typeshed/stdlib/pydoc.pyi:166:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:176:132: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:177:128: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:229:131: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:230:107: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:231:132: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:232:128: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/pydoc.pyi:233:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/tempfile.pyi:379:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5512 diagnostics
+ Found 5511 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] Improve @override and @final checks in cases where there are multiple reachable definitions" to "[ty] Improve `@override`, `@final` and Liskov checks in cases where there are multiple reachable definitions" by @AlexWaygood on 2025-12-02 23:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-02 23:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 00:06_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 1 | 11 | 0 |
| `invalid-method-override` | 9 | 0 | 0 |
| `invalid-argument-type` | 2 | 0 | 0 |
| **Total** | **12** | **11** | **0** |

**[Full report with detailed diff](https://alex-first-declaration.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-first-declaration.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-12-03 00:37_

Okay, the weird new Liskov violations on mypy's vendored version of typeshed are because e.g. ty thinks that the `ModuleSpec` definition in mypy's vendored version of `stdlib/importlib/_frozen_importlib.pyi` is a different class to the `ModuleSpec` definition in ty's vendored version of `stdlib/importlib/_frozen_importlib.pyi`. So "obviously" a class that overrides the a method that takes one `ModuleSpec` with a method that takes another `ModuleSpec` must violate the Liskov Subsitution Principle.

---

_Marked ready for review by @AlexWaygood on 2025-12-03 00:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-03 00:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-03 00:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-03 00:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-03 00:44_

---

_@AlexWaygood reviewed on 2025-12-03 00:46_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1 on 2025-12-03 00:46_

@BurntSushi -- both of the tests being changed in this file have FIXME comments above them. But I'm not sure if the changes this PR makes to the tests fixes the problems, or makes the problems worse 😆 I'm not totally sure I understand what these tests are meant to be asserting

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/final.md`:498 on 2025-12-03 06:30_

It seems like we do? At least at the first definition...

---

_@carljm approved on 2025-12-03 06:38_

Looks good!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:498 on 2025-12-03 11:46_

no, we emit a diagnostic stating that an `@final` method has been overridden (which is not to my mind a Liskov violation), but we are silent about the fact that the _type_ has also been incompatibly overridden (which is, to my mind, a Liskov violation)

---

_@AlexWaygood reviewed on 2025-12-03 11:46_

---

_@AlexWaygood reviewed on 2025-12-03 12:16_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1 on 2025-12-03 12:16_

LMK if you consider this a regression and I can fix it in a followup. It shouldn't be too hard to track whether a member has multiple definitions and propagate that information upwards, if that's what the autocomplete machinery wants to know

---

_Merged by @AlexWaygood on 2025-12-03 12:51_

---

_Closed by @AlexWaygood on 2025-12-03 12:51_

---

_Branch deleted on 2025-12-03 12:51_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:1 on 2025-12-03 13:54_

I think the idea here was just trying to test what happens when we try to insert an import for a symbol that is already imported, but whose imports are conditional. I think in this case we don't want to add any new imports. But we weren't doing that before either. So I think this is fine for now.

---

_@BurntSushi reviewed on 2025-12-03 13:54_

---

_@AlexWaygood reviewed on 2025-12-03 13:56_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/importer.rs`:1 on 2025-12-03 13:56_

cool, thank you!

---

```yaml
number: 21792
title: "[ty] Support renaming import aliases"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/rename-import-alias
created_at: 2025-12-04T14:26:16Z
updated_at: 2025-12-05T18:23:49Z
url: https://github.com/astral-sh/ruff/pull/21792
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Support renaming import aliases

---

_@MichaReiser_

## Summary

Fixes renaming import aliases or symbols that include an import alias in their chain.

For renames (and highlight references), we mustn't resolve to the final declaration. Instead, we should only resolve to the first declaration that defined this specific name (e.g. the import alias). We already made this distinction internally but we didn't correctly propagate the `alias_resolution`, which resulted in some definitions resolving to the final declarations and others only resolved to the first import alias. 

It's fairly likely that we still fail to propagate `ImportAliasResolution` in some places but we can fix those as they come up.

This PR fixes this.

I also decided to split `GotoTarget::ImportSymbolAlias` into two variants, one for when the target is the alias's `asname` and one where it's the alias's name. Mainly because I found the distinction only based on `range` hard to follow and this also revealed some further bugs.

Fixes https://github.com/astral-sh/ty/issues/1661

## Test plan


https://github.com/user-attachments/assets/0b825f2e-8ab5-48f5-adc8-c1d4e0e59f22



---

_Label `server` added by @MichaReiser on 2025-12-04 14:26_

---

_Label `ty` added by @MichaReiser on 2025-12-04 14:26_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 14:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 14:30_


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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 38 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5518 diagnostics
+ Found 5517 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:512 on 2025-12-04 15:21_

I need to add a test for this. I think this might break `a.call` if `ImportAliasResolution::PreserveAlias`, but `definitions_for_callable` always resolves import aliases (it doesn't perform the lookup by name).

---

_@MichaReiser reviewed on 2025-12-04 15:21_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:82 on 2025-12-04 16:16_

Incorrect span

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:394 on 2025-12-04 16:17_

Yikes great point

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:514 on 2025-12-04 16:21_

I need to remember the dbg! macro more

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:624 on 2025-12-04 16:24_

Flagging this if-let as suspicious (we discussed this, assuming you know the conclusion better than I do).

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:920 on 2025-12-04 16:25_

This should have the asname directly now

---

_@Gankra reviewed on 2025-12-04 16:26_

---

_@MichaReiser reviewed on 2025-12-04 18:05_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:1081 on 2025-12-04 18:05_

This looks wrong

---

_Comment by @codspeed-hq[bot] on 2025-12-04 18:22_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Frename-import-alias?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21792 will **not alter performance**

<sub>Comparing <code>micha/rename-import-alias</code> (adf468b) with <code>main</code> (b2fb421)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Frename-import-alias?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @MichaReiser on 2025-12-05 10:54_

---

_Review requested from @carljm by @MichaReiser on 2025-12-05 10:54_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-05 10:54_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-05 10:54_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-05 10:54_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-05 10:54_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-05 10:54_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-12-05 10:54_

---

_@MichaReiser reviewed on 2025-12-05 13:54_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:40 on 2025-12-05 13:54_

We might want to unify `ReferencesMode` and `ImportAliasResolution` because I think there might be more differences in `resolve_*` moving forward where it might even be beneficial to know: Hey, what request are we handling, it's rename, cool, let's do this instead of x.

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:516 on 2025-12-05 15:16_

Concretely: the reason GotoTarget::Call exists if for when you click on `Class()`. `definitions_for_expression` will find `class Class` while `definitions_for_callable` will find `def __init__()`. So indeed we want to exclude`definitions_for_callable` when we don't want to ResolveAliases!

---

_Review comment by @Gankra on `crates/ty_ide/src/rename.rs`:28 on 2025-12-05 15:23_

Should we be using `ReferencesMode::to_import_resolution`?

---

_Review comment by @Gankra on `crates/ty_ide/src/rename.rs`:1249 on 2025-12-05 15:26_

the absolute confidence of a ruff dev to be like "yeah of course that cursor is on deprecated"

---

_@Gankra approved on 2025-12-05 15:29_

Hmm is this rebased on main? I think some from..import tests I added should also have had snapshot updates.

---

_@MichaReiser reviewed on 2025-12-05 16:22_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:1249 on 2025-12-05 16:22_

I debugged way too many `TextRange` problems :)

---

_@MichaReiser reviewed on 2025-12-05 16:22_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:516 on 2025-12-05 16:22_

Ohh, thank you! That makes sense

---

_Comment by @MichaReiser on 2025-12-05 18:02_

> Hmm is this rebased on main? I think some from..import tests I added should also have had snapshot updates.


I rebased a couple of times today. Any specific examples? I don't see any other `rename` tests that use an alias. 

I did see some changes to your submodule import tests when trying to fix overload resolution that looked related to redeclarations. 

---

_Comment by @MichaReiser on 2025-12-05 18:06_

I'll go ahead and merge this. Happy to address any tests that should have been updated but didn't in a follow up PR

---

_Merged by @MichaReiser on 2025-12-05 18:12_

---

_Closed by @MichaReiser on 2025-12-05 18:12_

---

_Branch deleted on 2025-12-05 18:12_

---

_Comment by @Gankra on 2025-12-05 18:23_

Ah good point, the cases I got in are specifically failing on non-aliases, so no worries.

---

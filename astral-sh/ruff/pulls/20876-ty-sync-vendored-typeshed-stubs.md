```yaml
number: 20876
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-10-15T00:40:02Z
updated_at: 2025-10-15T09:13:35Z
url: https://github.com/astral-sh/ruff/pull/20876
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-10-15 00:40_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2025-10-15 00:40_

---

_Review requested from @carljm by @github-actions[bot] on 2025-10-15 00:40_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-10-15 00:40_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-10-15 00:40_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-10-15 00:40_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-10-15 00:40_

---

_Label `ty` added by @github-actions[bot] on 2025-10-15 00:40_

---

_Closed by @sharkdp on 2025-10-15 05:10_

---

_Reopened by @sharkdp on 2025-10-15 05:10_

---

_Comment by @github-actions[bot] on 2025-10-15 05:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-15 08:46:46.391831607 +0000
+++ new-output.txt	2025-10-15 08:46:49.708854720 +0000
@@ -1,5 +1,5 @@
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d017)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1643f)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16443)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-15 05:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
setuptools (https://github.com/pypa/setuptools)
+ setuptools/_vendor/inflect/__init__.py:2279:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ setuptools/_vendor/inflect/__init__.py:2283:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 799 diagnostics
+ Found 801 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- scripts/contrib-patch-tests.py:34:69: error[unresolved-attribute] Type `expr` has no attribute `id`
- Found 7578 diagnostics
+ Found 7577 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-15 07:09_

---

_Comment by @github-actions[bot] on 2025-10-15 07:17_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 1 | 0 |
| **Total** | **2** | **1** | **0** |

**[Full report with detailed diff](https://typeshedbot-sync-typeshed.ecosystem-663.pages.dev/diff)** ([timing results](https://typeshedbot-sync-typeshed.ecosystem-663.pages.dev/timing))


---

_@sharkdp reviewed on 2025-10-15 07:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/union.md`:87 on 2025-10-15 07:49_

This is due to https://github.com/python/typeshed/pull/14813. It's not technically incorrect, but breaks in cases that previously relied on the `@Todo` type to shadow false positives. We may need to special-case `type.__or__`'s return type (for now)?

---

_@sharkdp reviewed on 2025-10-15 07:52_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:977 on 2025-10-15 07:52_

This also "breaks" because we previously inferred a `@Todo` type, and `@Todo` types are generally hidden ... (why?)

---

_@AlexWaygood reviewed on 2025-10-15 07:59_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:977 on 2025-10-15 07:59_

Only specifically TypeAlias-related TODOs are suppressed from autocompletions: https://github.com/astral-sh/ruff/blob/4fc7dd300c493fc16ea279928a2ad353e070c92e/crates/ty_python_semantic/src/types/ide_support.rs#L272

We never want private type aliases appearing in autocompletions. The special Todo variant was the best way I could think of doing this without PEP-613 support

---

_@AlexWaygood reviewed on 2025-10-15 08:01_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:977 on 2025-10-15 08:01_

Instances of UnionType are also suppressed, because they're almost certainly implicit type aliases, and we don't support them yet either, but we never want private type aliases from stubs appearing in autocompletions because they don't exist at runtime: https://github.com/astral-sh/ruff/blob/4fc7dd300c493fc16ea279928a2ad353e070c92e/crates/ty_python_semantic/src/types/ide_support.rs#L262

---

_@AlexWaygood reviewed on 2025-10-15 08:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/union.md`:87 on 2025-10-15 08:09_

> We may need to special-case `type.__or__`'s return type (for now)?

That sounds reasonable. Sorry I didn't give any warning that this was coming — completely forgot about all the special casing we have for PEP-604 unions and type aliases when I merged that typeshed PR :-(

It might be easiest to work in the special case at a higher level, in inference of `|` operations in `infer/builder.rs`

---

_@sharkdp reviewed on 2025-10-15 08:21_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:977 on 2025-10-15 08:21_

> Instances of UnionType are also suppressed

This is why it breaks, because now it's `UnionType | <class 'int'>`.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-15 08:46_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-15 08:46_

---

_@AlexWaygood approved on 2025-10-15 08:51_

Thank you!

---

_@sharkdp reviewed on 2025-10-15 08:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/union.md`:87 on 2025-10-15 08:53_

> Sorry I didn't give any warning that this was coming — completely forgot about all the special casing we have for PEP-604 unions and type aliases when I merged that typeshed PR :-(

We certainly didn't expect you to. This also looks innocent enough :smiley:

---

_Merged by @sharkdp on 2025-10-15 09:13_

---

_Closed by @sharkdp on 2025-10-15 09:13_

---

_Branch deleted on 2025-10-15 09:13_

---

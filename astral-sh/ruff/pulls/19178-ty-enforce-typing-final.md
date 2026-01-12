```yaml
number: 19178
title: "[ty] Enforce `typing.Final`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/enforce-final
created_at: 2025-07-07T14:03:19Z
updated_at: 2025-07-10T12:20:01Z
url: https://github.com/astral-sh/ruff/pull/19178
synced_at: 2026-01-12T15:56:33Z
```

# [ty] Enforce `typing.Final`

---

_@sharkdp_

## Summary

Emit a diagnostic when a `Final`-qualified symbol is modified. This first iteration only works for name targets. Tests with TODO comments were added for attribute assignments as well.

related ticket: https://github.com/astral-sh/ty/issues/158

## Ecosystem impact

Correctly identified [modification of a `Final` symbol](https://github.com/sphinx-doc/sphinx/blob/7b4164a5f2b64781475e64daf2d05cce2a0261d8/sphinx/__init__.py#L44) (behind a `# type: ignore`):
```diff
- warning[unused-ignore-comment] sphinx/__init__.py:44:56: Unused blanket `type: ignore` directive
```
And the same [here](https://github.com/python-trio/trio/blob/5471a37e82b36f556e0d26b36cb95a6b05afbef1/src/trio/_core/_run.py#L128):
```diff
- warning[unused-ignore-comment] src/trio/_core/_run.py:128:45: Unused blanket `type: ignore` directive
```

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-07-07 14:03_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-07 14:06_

---

_Comment by @github-actions[bot] on 2025-07-07 14:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- warning[unused-ignore-comment] src/trio/_core/_run.py:128:45: Unused blanket `type: ignore` directive
- Found 799 diagnostics
+ Found 798 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- warning[unused-ignore-comment] sphinx/__init__.py:44:56: Unused blanket `type: ignore` directive
- Found 606 diagnostics
+ Found 605 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~63MB
+     memo fields = ~66MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~152MB
+     memo fields = ~159MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~316MB
+ TOTAL MEMORY USAGE: ~332MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~541MB
+     memo fields = ~568MB

```
</details>


---

_Marked ready for review by @sharkdp on 2025-07-08 11:12_

---

_Review requested from @carljm by @sharkdp on 2025-07-08 11:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-08 11:12_

---

_Review requested from @dcreager by @sharkdp on 2025-07-08 11:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:121 on 2025-07-08 13:02_

nit: maybe this might be a better message?

```suggestion
FINAL_A = 2  # error: [invalid-assignment] "Reassignment of `Final` symbol `FINAL_A` is not allowed"
```

since you're obviously allowed to assign to the `Final` symbol at the point where it's initially declared 😆

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:164 on 2025-07-08 13:07_

relevant discussion on this: https://discuss.python.org/t/imported-final-variable/82429

I sort-of disagree with the resolution there, but don't have the energy to relitigate the question now 😆 and it comes up very rarely. It's good for us to follow the behaviour the spec mandates, anyway (no change requested!).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:199 on 2025-07-08 13:13_

Yes -- pyre (and possibly pyrefly? Can't remember the exact status) has a feature called `ImmutableRef`, which they [presented on](https://youtu.be/7uixlNTOY4s?si=EcjO2lUf_vvx72iv&t=7816) at the typing summit this year. It's similar to `Final` but it also prevents any mutations to the symbol. It's a far more invasive and more complicated feature to implement; I'm not sure if it's ever going to be proposed for standardisation in the typing system.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1587 on 2025-07-08 13:19_

doesn't need to be done in this PR: should we add a `Place::is_definitely_bound()` method? It feels like we have a _lot_ of these `if matches!()` conditions around the codebase

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1666 on 2025-07-08 13:20_

Could we add a subdiagnostic pointing to the previous definition where it was declared as `Final`?

---

_@AlexWaygood approved on 2025-07-08 13:21_

🚀

---

_@sharkdp reviewed on 2025-07-08 14:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1666 on 2025-07-08 14:13_

Yes, thanks for the suggestion! It's hard to do this for imported symbols (unless @Gankra adds a vector of `Definition`s to `Place` :smile:), but I added a subdiagnostic for same-module `Final`s:

![image](https://github.com/user-attachments/assets/a897eaac-3ec1-49cb-bd78-9a90a9898c84)


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_`typing.Final`_-_Full_diagnostics_(174fdd8134fb325b).snap`:38 on 2025-07-08 14:16_

even better might be

```suggestion
3 | MY_CONSTANT: Final[int] = 1
  |              ---------- Symbol declared as `Final` here
4 |
5 | # more code
6 |
7 | MY_CONSTANT = 2  # error: [invalid-assignment]
  | ^^^^^^^^^^^^^^^ Symbol later reassigned here
```

but probably not worth spending time on that now if that's hard to achieve!

---

_@AlexWaygood approved on 2025-07-08 14:17_

---

_@sharkdp reviewed on 2025-07-08 14:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_`typing.Final`_-_Full_diagnostics_(174fdd8134fb325b).snap`:38 on 2025-07-08 14:25_

I tried to highlight the full range for the assignment, but for (annotated) assignments, `full_range` apparently only includes the left hand side target. I'll try to change that in a follow-up, since it might affect other diagnostics.

Highlighting the annotation for the original definition would also be nice, but is a bit harder because we would need to explicitly match on different definition kinds.

---

_Merged by @sharkdp on 2025-07-08 14:26_

---

_Closed by @sharkdp on 2025-07-08 14:26_

---

_Branch deleted on 2025-07-08 14:26_

---

_@sharkdp reviewed on 2025-07-08 14:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1587 on 2025-07-08 14:26_

will look into it

---

_@oconnor663 reviewed on 2025-07-09 18:30_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:150 on 2025-07-09 18:30_

This TODO isn't going to last long: rebasing https://github.com/astral-sh/ruff/pull/19112 on top of this PR makes this line fail :)

---

_@sharkdp reviewed on 2025-07-09 19:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:150 on 2025-07-09 19:15_

I added that just for you :gift: 

---

_@sharkdp reviewed on 2025-07-10 12:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1587 on 2025-07-10 12:08_

I looked into it, but it seems like such a function would only have this callsite. In most other cases, we also match on the type at the same time.

---

_@AlexWaygood reviewed on 2025-07-10 12:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1587 on 2025-07-10 12:20_

Oh, fair enough!

---

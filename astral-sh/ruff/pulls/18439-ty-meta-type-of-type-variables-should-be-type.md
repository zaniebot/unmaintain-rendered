```yaml
number: 18439
title: "[ty] Meta-type of type variables should be type[..]"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/typevar-meta-type
created_at: 2025-06-03T09:47:47Z
updated_at: 2025-06-04T07:50:17Z
url: https://github.com/astral-sh/ruff/pull/18439
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Meta-type of type variables should be type[..]

---

_@sharkdp_

## Summary

Came across this while debugging some ecosystem changes in https://github.com/astral-sh/ruff/pull/18347. I think the meta-type of a typevar-annotated variable should be equal to `type`, not `<class 'object'>`.

## Test Plan

New Markdown tests.


---

_Label `ty` added by @sharkdp on 2025-06-03 09:47_

---

_Comment by @github-actions[bot] on 2025-06-03 09:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- error[invalid-return-type] beartype/_util/utilobjmake.py:194:12: Return type does not match returned value: expected `T`, found `object`
- Found 573 diagnostics
+ Found 572 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ warning[unused-ignore-comment] pydantic/v1/fields.py:1049:49: Unused blanket `type: ignore` directive
- Found 760 diagnostics
+ Found 761 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ warning[unused-ignore-comment] src/hydra_zen/structured_configs/_implementations.py:1074:60: Unused blanket `type: ignore` directive
- error[invalid-return-type] src/hydra_zen/structured_configs/_implementations.py:1314:20: Return type does not match returned value: expected `_T`, found `object`

```
</details>


---

_Label `bug` added by @sharkdp on 2025-06-03 10:10_

---

_Marked ready for review by @sharkdp on 2025-06-03 10:14_

---

_Review requested from @carljm by @sharkdp on 2025-06-03 10:14_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-03 10:14_

---

_Review requested from @dcreager by @sharkdp on 2025-06-03 10:14_

---

_@AlexWaygood approved on 2025-06-03 10:17_

Makes sense to me! The implicit upper bound of a TypeVar that does not have bounds or constraints is `object`, and the meta-type of `object` is `type`.

(tiny nit: I would spell it "meta-type" rather than "meta type")

---

_Comment by @sharkdp on 2025-06-03 11:43_

> Makes sense to me! The implicit upper bound of a TypeVar that does not have bounds or constraints is `object`.

Yes: That's also why I added that second test: to show that an unbounded `T` behaves the same way as `T: object` :+1: 

---

_Renamed from "[ty] Meta type of type variables should be type[..]" to "[ty] Meta-type of type variables should be type[..]" by @sharkdp on 2025-06-03 11:44_

---

_Merged by @sharkdp on 2025-06-03 13:22_

---

_Closed by @sharkdp on 2025-06-03 13:22_

---

_Branch deleted on 2025-06-03 13:22_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md`:772 on 2025-06-03 13:23_

Can we add the same stanza of tests to `legacy/variables.md` too?  (I don't think this particular code path would be effected, but we have seen tests that pass/fail inconsistently depending on which typevar flavor you use)

---

_@dcreager reviewed on 2025-06-03 13:23_

---

_@sharkdp reviewed on 2025-06-03 13:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md`:772 on 2025-06-03 13:32_

Yes. I saw that second file and wasn't really sure if it was worth duplicating the tests. Should have asked instead of assuming it was fine to only do the PEP-695 variant. I'll do a follow-up.

---

_@sharkdp reviewed on 2025-06-04 07:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md`:772 on 2025-06-04 07:50_

https://github.com/astral-sh/ruff/pull/18453

---

```yaml
number: 21828
title: "[ty] Avoid double-analyzing tuple in `Final` subscript"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/sli
created_at: 2025-12-06T22:26:14Z
updated_at: 2025-12-07T14:27:15Z
url: https://github.com/astral-sh/ruff/pull/21828
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Avoid double-analyzing tuple in `Final` subscript

---

_Pull request opened by @charliermarsh on 2025-12-06 22:26_

## Summary

As-is, a single-element tuple gets destructured via:

```rust
let arguments = if let ast::Expr::Tuple(tuple) = slice {
    &*tuple.elts
} else {
    std::slice::from_ref(slice)
};
```

But then, because it's a single element, we call `infer_annotation_expression_impl`, passing in the tuple, rather than the first element.

Closes https://github.com/astral-sh/ty/issues/1793.
Closes https://github.com/astral-sh/ty/issues/1768.


---

_@charliermarsh reviewed on 2025-12-06 22:27_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder/annotation_expression.rs`:331 on 2025-12-06 22:27_

We want to analyze the single element, rather than the slice. (They're the same thing if it's not a single-element tuple.)

---

_Marked ready for review by @charliermarsh on 2025-12-06 22:27_

---

_Review requested from @carljm by @charliermarsh on 2025-12-06 22:27_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-06 22:27_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-06 22:27_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-06 22:27_

---

_Renamed from "Avoid double-analyzing tuple in `Final` subscript" to "[ty] Avoid double-analyzing tuple in `Final` subscript" by @charliermarsh on 2025-12-06 22:27_

---

_Label `ty` added by @charliermarsh on 2025-12-06 22:27_

---

_Label `bug` added by @charliermarsh on 2025-12-06 22:27_

---

_Comment by @astral-sh-bot[bot] on 2025-12-06 22:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-06 22:30_


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

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5519 diagnostics
+ Found 5518 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:115 on 2025-12-07 13:22_

Let's also add tests to show:
1. That we still infer it as a `ClassVar` even though the type argument is invalid
2. What we infer the type as being, given the invalid annotation

```suggestion
class C:
    # error: [invalid-type-form] "Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?"
    x: ClassVar[(),]

# error: [invalid-attribute-access] "Cannot assign to ClassVar `X` from an instance of type `Foo`"
C().x = 42
reveal_type(C.x)  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:352 on 2025-12-07 13:27_

similar here

```suggestion
# error: [invalid-type-form] "Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?"
x: Final[(),] = 42

# error: [invalid-assignment] "Reassignment of `Final` symbol `x` is not allowed"
x = 56
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:107 on 2025-12-07 13:28_

this isn't rendered very well by GitHub right now: https://github.com/astral-sh/ruff/blob/charlie/sli/crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md#trailing-comma-creates-a-tuple

```suggestion
gracefully and emit a proper error rather than crashing (see [ty#1793](https://github.com/astral-sh/ty/issues/1793)).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:346 on 2025-12-07 13:29_

```suggestion
gracefully and emit a proper error rather than crashing (see [ty#1793](https://github.com/astral-sh/ty/issues/1793)).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:116 on 2025-12-07 13:29_

```suggestion
gracefully and emit a proper error rather than crashing (see [ty#1793](https://github.com/astral-sh/ty/issues/1793)).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:125 on 2025-12-07 13:32_

we could add a test here too to show that we still understand that `x` is an `InitVar` (it contributes to the `__init__` signature but is not available as an attribute on instances), despite the illegal type parameter:

```suggestion
@dataclass
class AlsoWrong:
    # error: [invalid-type-form] "Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?"
    x: InitVar[(),]
    
# revealed: (self: AlsoWrong, x: Unknown) -> None
reveal_type(AlsoWrong.__init__)

# error: [unresolved-attribute]
reveal_type(AlsoWrong().x)  # revealed: Unknown
```

---

_@AlexWaygood approved on 2025-12-07 13:35_

Great fix!!

I feel like some of the "Did you mean `tuple[()]`?" suggestions are misfiring a bit here, but this is a real edge case so it probably doesn't matter that much. The main thing is to make sure we don't panic

---

_Comment by @AlexWaygood on 2025-12-07 13:36_

This PR also fixes https://github.com/astral-sh/ty/issues/1768, so you could add a test case that includes the MRE from that issue

---

_Comment by @charliermarsh on 2025-12-07 14:21_

> I feel like some of the "Did you mean tuple[()]?" suggestions are misfiring a bit here.

Yeah I had the same reaction. I did very that (at least) that code doesn't crash at runtime though...


---

_Merged by @charliermarsh on 2025-12-07 14:27_

---

_Closed by @charliermarsh on 2025-12-07 14:27_

---

_Branch deleted on 2025-12-07 14:27_

---

```yaml
number: 21833
title: "[ty] Fix stack overflows when attempting to infer the truthiness of a TypeVar with an invalid recursive bound or constraints"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: alex/truthiness-panic
created_at: 2025-12-07T17:59:24Z
updated_at: 2025-12-09T18:03:57Z
url: https://github.com/astral-sh/ruff/pull/21833
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Fix stack overflows when attempting to infer the truthiness of a TypeVar with an invalid recursive bound or constraints

---

_@AlexWaygood_

## Summary

This PR fixes the first stack overflow MRE given in https://github.com/astral-sh/ty/issues/1794.

## Test Plan

I added two corpus test cases that cause us to overflow our stack on `main`: one with a recursive upper bound, and a slightly trickier one with recursive constraints. I also added mdtests and snapshots.


---

_Review requested from @carljm by @AlexWaygood on 2025-12-07 17:59_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-07 17:59_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-07 17:59_

---

_Label `bug` added by @AlexWaygood on 2025-12-07 17:59_

---

_Label `ty` added by @AlexWaygood on 2025-12-07 17:59_

---

_Comment by @astral-sh-bot[bot] on 2025-12-07 18:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@AlexWaygood reviewed on 2025-12-07 18:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5428 on 2025-12-07 18:01_

I first naively tried doing this

```suggestion
                    Some(TypeVarBoundOrConstraints::Constraints(constraints)) => {
                        visitor.visit(*self, || try_union(constraints.as_type(db)))?
                    }
```

but that's no good -- the `TypeVarConstraints::as_type()` call itself causes a stack overflow if one of the TypeVar constraints is recursive. So I had to refactor `try_union` so that it could take a `UnionType` or a `TypeVar`.

---

_Comment by @astral-sh-bot[bot] on 2025-12-07 18:03_


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

_Comment by @AlexWaygood on 2025-12-08 10:20_

#21840 also fixes this issue and looks like it might do so in a better way (though I haven't studied that PR in detail yet)

---

_Comment by @mtshiba on 2025-12-08 11:09_

> #21840 also fixes this issue and looks like it might do so in a better way (though I haven't studied that PR in detail yet)

What #21840 does is report the type variable that creates the "false" recursion as an invalid type variable and mark its specialization as `Unknown`.

Neither of the panic cases detected by the fuzzer is a valid recursive type variable.

```python
# name_2[0] => Unknown
class name_1[name_2: name_2[0]]:
    def name_4(name_3: name_2, /):
        if name_3:
            pass

#  (name_5 if unique_name_0 else name_1)[0] => Unknown
def name_4[name_5: (name_5 if unique_name_0 else name_1)[0], **name_1](): ...
```

Therefore, it cannot handle stack overflows that occur with truly valid recursive type variables.


---

_Comment by @AlexWaygood on 2025-12-08 15:20_

> Therefore, it cannot handle stack overflows that occur with truly valid recursive type variables.

Right, but I'm struggling to come up with any examples of stack overflows with truly valid recursive type variables 😆 are you able to find any?

---

_Converted to draft by @AlexWaygood on 2025-12-08 15:20_

---

_Comment by @mtshiba on 2025-12-09 17:58_

> Right, but I'm struggling to come up with any examples of stack overflows with truly valid recursive type variables 😆 are you able to find any?

Nah, I think the stack overflow is probably caused by an incorrect specialization. The specialization `T[0]` doesn't make any sense, but ty treats it as `T`.

```python
def f[T: (int, list[T])](x: T):
    if not isinstance(x, int):
        reveal_type(x)  # list[typing.TypeVar]

def f[T: (int, list[T[0]])](x: T):
    if not isinstance(x, int):
        reveal_type(x)  # list[T]
```

---

_Comment by @mtshiba on 2025-12-09 17:58_

I re-read PEP-695, and it appears that creating recursive type variables is not permitted by the type system in the first place (even though it's possible at runtime).

https://peps.python.org/pep-0695/#type-parameter-scopes

> ```python
> # The following generates no compiler error, but a type checker
> # should generate an error because an upper bound type must be concrete,
> # and ``Sequence[S]`` is generic. Future extensions to the type system may
> # eliminate this limitation.
> class ClassA[S, T: Sequence[S]]: ...
>
> # The following generates no compiler error, because the bound for ``S``
> # is lazily evaluated. However, type checkers should generate an error.
> class ClassB[S: Sequence[T], T]: ...
> ```

The requirement that the bound of a type variable be concrete seems to imply that it is forbidden to use itself within it.

Therefore, I think it would be compliant to treat recursions found in type variables as errors and replace them with `Unknown`.
It seems odd that recursive type aliases and protocols are allowed but type variables are not, but that's the specification.

---

_Comment by @AlexWaygood on 2025-12-09 18:03_

> It seems odd that recursive type aliases and protocols are allowed but type variables are not, but that's the specification.

I think the reason for that is that it opens the door to higher-kinded types, which would be a very significant development in the Python type system and would require a lot of design work.

Thank you!

---

_Closed by @AlexWaygood on 2025-12-09 18:03_

---

_Branch deleted on 2025-12-09 18:03_

---

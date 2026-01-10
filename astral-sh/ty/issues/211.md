```yaml
number: 211
title: "Bare `ClassVar` annotations"
type: issue
state: closed
author: sharkdp
labels:
  - typing semantics
assignees: []
created_at: 2025-01-27T10:30:01Z
updated_at: 2025-07-07T13:04:29Z
url: https://github.com/astral-sh/ty/issues/211
synced_at: 2026-01-10T02:07:35Z
```

# Bare `ClassVar` annotations

---

_Issue opened by @sharkdp on 2025-01-27 10:30_

### Description

Consider a 'bare' `ClassVar` annotation, i.e. without an explicit `ClassVar[…]` type argument:
```py
class C:
    var: ClassVar = 1
```

There are two questions here:
* Should this be allowed?
* If allowed, what should the public type of `C.var` be?

## Should this be allowed?

Arguments for why it should be disallowed:

* [PEP 526](https://peps.python.org/pep-0526/#class-and-instance-variable-annotations) which introduced `ClassVar` doesn't mention the possibility of a bare `ClassVar`
* The [typing spec](https://typing.readthedocs.io/en/latest/spec/class-compat.html#classvar) seems rather clear that this is not allowed:
  
  > A [type qualifier](https://typing.readthedocs.io/en/latest/spec/glossary.html#term-type-qualifier) `ClassVar[T]` exists in the [typing](https://docs.python.org/3/library/typing.html#module-typing) module. It accepts only a single argument that should be a valid type, and is used to annotate class variables that should not be set on class instances.
* A bare [`Final` is *explicitly allowed*](https://typing.readthedocs.io/en/latest/spec/qualifiers.html#syntax) in the spec, but no such clarification exists for `ClassVar`.
* The [syntax for type and annotations expressions](https://typing.readthedocs.io/en/latest/spec/annotations.html#type-and-annotation-expressions) does not seem to allow for a `ClassVar` without bracketed arguments, because it would fall under the `type_expression` branch and this only allows bare names if they *"refer to a valid in-scope class, type alias, or TypeVar"*, but `ClassVar` is a special form:
   ```
  annotation_expression ::=  …
                             | <ClassVar> '[' annotation_expression ']'
                             | <Final> ('[' annotation_expression']')?
                             | …
                             | type_expression

  type_expression       ::=  …
                             | name
                                   (where name must refer to a valid in-scope class,
                                    type alias, or TypeVar)
                             | …
  ```
  ~~Although to be fair, the syntax does not seem to allow a bare `Final` either, which is explicitly allowed in the spec~~ This [has actually been changed](https://github.com/python/typing/pull/1915). A bare `Final` is now explicitly allowed.

Arguments for why it should be allowed:

* It is okay to declare the type of a (class) variable using `var: int`, or not to declare it at all. It therefore seems reasonable to allow both `var: ClassVar[int]` and `var: ClassVar`, because `ClassVar` is a type *qualifier* that adds information that is orthogonal to the declared type.
* Mypy and Pyright both support a bare `ClassVar` annotation in the sense that they raise a diagnostic if you try to modify `var` through an instance.
* `ClassVar` seems similar in spirit to `Final`, and a bare `Final` is explicitly allowed (but see below for why we probably *don't* want a bare `ClassVar` to have the same implication as a bare `Final`).

## If allowed, what should the public type of `C.var` be?

A bare `Final` has the following [meaning](https://typing.readthedocs.io/en/latest/spec/qualifiers.html#syntax):

> ```py
> ID: Final = 1
> ```
> The typechecker should apply its usual type inference mechanisms to determine the type of `ID` (here, likely, `int`). Note that unlike for generic classes this is not the same as `Final[Any]`.

This suggests that the type should be inferred from the right-hand side of the definition. For Red Knot, this would mean that we infer `Literal[1]` which is both precise and correct for `Final` variables, as they can not be modified.

But for `ClassVar`, this seems undesirable. If we treat `C.var` from above as having type `Literal[1]`, we would not be allowed to modify it. There are two other reasonable behaviors:

1. Use `Unknown | Literal[1]` as the public type, which would also be the type of an un-annotated class variable
2. Use `Unknown` as the public type, which would also be the public type of a class variable annotated with `var: ClassVar[Unknown]`

Option 1 is the behavior that is documented as being desirable in TODO comments ([[1]](https://github.com/astral-sh/ruff/blob/2ef94e5f3e6e57a19151334b598e2bf381e97dac/crates/red_knot_python_semantic/resources/mdtest/type_qualifiers/classvar.md?plain=1#L24-L25), [[2]](https://github.com/astral-sh/ruff/blob/2ef94e5f3e6e57a19151334b598e2bf381e97dac/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L186-L187)). Option 2 is our current behavior on `main`.

Implementing Option 1 is quite involved, as there is a difference (inconsistency?) between undeclared variables and variables declared with `Unknown` (as documented [here](https://github.com/astral-sh/ruff/blob/2ef94e5f3e6e57a19151334b598e2bf381e97dac/crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md#undeclared-but-bound). It would probably involve returning `Option<Type<…>>` instead of `Type<…>` in functions like `infer_annotation_expression`. It might also involve treating `var: ClassVar = 1` as a pure binding?

If Option 2 seems like a possible alternative (for now?), we can simply remove some TODO comments (draft PR [here](https://github.com/astral-sh/ruff/pull/15768)).

---

_Comment by @sharkdp on 2025-01-31 10:15_

Interestingly, the [type annotation/expression syntax](https://typing.readthedocs.io/en/latest/spec/annotations.html#type-and-annotation-expressions) has been [changed](https://github.com/python/typing/pull/1915) to explicitly allow a bare `Final`:
```diff
-                         : | <Final> '[' `annotation_expression`']'
+                         : | <Final> ('[' `annotation_expression`']')?
```

---

_Comment by @sharkdp on 2025-03-01 19:48_

It looks like the spec will be changed to explicitly allow a bare `ClassVar`: https://github.com/python/typing/pull/1931

---

_Renamed from "[red-knot] Bare `ClassVar` annotations" to "Bare `ClassVar` annotations" by @MichaReiser on 2025-05-07 15:27_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:49_

---

_Closed by @sharkdp on 2025-07-07 13:04_

---

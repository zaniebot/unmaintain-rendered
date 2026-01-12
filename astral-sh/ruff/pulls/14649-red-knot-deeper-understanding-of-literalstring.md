```yaml
number: 14649
title: "[red-knot] Deeper understanding of `LiteralString`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-literalstring
created_at: 2024-11-28T00:11:02Z
updated_at: 2024-12-04T18:58:18Z
url: https://github.com/astral-sh/ruff/pull/14649
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Deeper understanding of `LiteralString`

---

_@InSyncWithFoo_

## Summary

Resolves #14648.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-11-28 00:11_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-11-28 00:11_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-11-28 00:11_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-11-28 00:11_

---

_Comment by @github-actions[bot] on 2024-11-28 00:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:3 on 2024-11-28 03:22_


```suggestion
`LiteralString` represents a string that is either defined directly within the source code or is
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:10 on 2024-11-28 03:42_

IMO this section goes too far. We don't need to test that every special annotation form works in every possible type expression location. It's sufficient to test that it works in a type expression, and we will have other tests showing that all these various locations are parsed as type expressions, as we add support for them.

Also, this section doesn't go far enough :) In that the tests here don't really test that we actually understand the annotation. I expect most or all of these tests already pass without the code changes in this PR?

I would simplify this entire section down to a single test showing that `x: LiteralString` results in a declared type of `LiteralString` for `x`. Unless you think there are specific other cases that are especially interesting to test (i.e. because it would be easy to get them wrong in the implementation.) The test can look something like this:

```py
x: LiteralString

def f():
    reveal_type(x)  # revealed: LiteralString
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:104 on 2024-11-28 03:46_

It's not clear to me what these have to do with parametrizing `LiteralString`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:100 on 2024-11-28 03:47_

I don't think there's anything we need to test here involving `TypeAlias` or `TypeAliasType`, which would also eliminate the need for the TODO

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:42 on 2024-11-28 03:48_

IMO these two lines are the only thing we actually need to test in the tests for `LiteralString`. The rest belongs in a future test suite for type aliases, which we don't really support yet.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:160 on 2024-11-28 03:53_

I don't think we should likely ever add the level of special-casing that would be needed to infer `"foo, bar"` here. I think we should just follow the typeshed annotations (and overloads) for `str.format`, which would result in inferring this as `LiteralString`. (That's also a more appropriate thing to test in this file.)
```suggestion
# TODO: Infer `LiteralString`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:164 on 2024-11-28 03:54_

"Compatible" is an informal term that isn't defined in the Python typing spec. We should prefer defined terms, such as "assignable" or "subtype"
```suggestion
### Assignability
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:167 on 2024-11-28 03:55_

```suggestion
`Literal[""]` is assignable to `LiteralString`, and `LiteralString` is assignable to `str`, but
not vice versa.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:176 on 2024-11-28 03:56_

```suggestion
foo_2 = "foo" if bool() else "bar"
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:192 on 2024-11-28 03:56_

```suggestion
baz_3 = "foo" if bool() else 1
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:636 on 2024-11-28 04:01_

All of the tests in this PR pass without adding these lines. The default fallback case for `is_subtype_of` is "not a subtype", so generally we don't need to add specific `false` cases like this, unless they need to be handled before a subsequent "true" case.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1411 on 2024-11-28 04:02_

"call type" doesn't really make sense here, "call todo" would make more sense
```suggestion
            Type::Todo(_) => CallOutcome::callable(todo_type!("call todo")),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:302 on 2024-11-28 04:06_

This looks like it's improving support for chained comparisons? It doesn't appear directly connected to `LiteralString`.

I'm not immediately seeing what test added in this PR requires this support?

Unless this is definitely required in this PR for reasons I'm not seeing, I think it would be easier to evaluate in a separate PR, accompanied by tests which specifically require this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:341 on 2024-11-28 04:09_

Building an intersection with just one type in it is a no-op, just use the single type directly.

```suggestion
                            constraints.insert(symbol, rhs_ty);
```

---

_@carljm requested changes on 2024-11-28 04:10_

Thanks for the contribution! I appreciate the thorough tests, but it is possible to have too many tests :) We should make sure that each test, and each part of each test, is asserting something meaningful about the added feature. Usually added tests should fail without the code added in the PR, unless the PR is specifically about improving test coverage for existing features.

---

_Label `red-knot` added by @MichaReiser on 2024-11-28 06:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1411 on 2024-11-28 08:28_

We could also just propagate the existing TODO from the left-hand side

---

_@AlexWaygood reviewed on 2024-11-28 08:28_

---

_@InSyncWithFoo reviewed on 2024-11-28 13:17_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:10 on 2024-11-28 13:17_

Most of the logic is already in place (props to everyone!), so, yes, this PR is rather small in terms of features. Removed most of the tests.

---

_@InSyncWithFoo reviewed on 2024-11-28 13:17_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:160 on 2024-11-28 13:17_

I have to disagree on this one. While it is true that the type checker often doesn't need to care about such minutiae, the linter might, especially if we are going to add support for embedded languages in the future.

Consider this regex example:

```python
# Regex group names, extracted to constants to avoid typos
NAME = 'name'      # Inferred type: Final[Literal['name']]
DOMAIN = 'domain'  # Inferred type: Final[Literal['domain']]

# This should work as if it was written as a literal string.
PATTERN = re.compile('(?P<{}>[^@]+)@(?P<{}>[^@]+)'.format(NAME, DOMAIN))

# Somewhere else
match = PATTERN.fullmatch(email)
name, group = match[NAME], match[DOMAIN]
```

---

_@InSyncWithFoo reviewed on 2024-11-28 13:18_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:176 on 2024-11-28 13:18_

Red Knot's inference capabilities are so powerful, an `if` expression won't prevent it from knowing that `foo_2` will be `"bar"`. These two ugly branches are thus necessary to ensure that `foo_2` is inferred to be of type `Literal["foo", "bar"]`.

---

_@InSyncWithFoo reviewed on 2024-11-28 13:19_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types/narrow.rs`:302 on 2024-11-28 13:19_

This part is covered by the "Narrowing" section. `last_rhs_ty` is introduced to avoid inferring an expression twice. Added a chained comparison test.

---

_@AlexWaygood reviewed on 2024-11-28 13:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:176 on 2024-11-28 13:21_

This is because you're using `bool()` to create the test, and red-knot knows that `bool()` with no arguments creates an object of type `Literal[False]`. It actually seems like a bug that it _doesn't_ know that for the more verbose form that doesn't use a ternary expression. We shouldn't rely on the bug.

Instead, you should create a custom function that is annotated as returning a `bool`, since red-knot will not be able to "see through" the return annotation, and use _that_ for the test:

```py
def coinflip() -> bool:
    return True

foo_2 = "foo" if coinflip() else "bar"
```

---

_@AlexWaygood reviewed on 2024-11-28 13:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:160 on 2024-11-28 13:30_

I see your point @InSyncWithFoo: maybe string templating really is special enough for us to warrant special-casing `str.format` in the way you describe here. It's a somewhat slippery slope there, though (we obviously are not building a complete Python interpreter in this project!), and I feel like it's not clear that we'll _necessarily_ add this special casing. It's also going to be a long way off; adding this kind of special casing isn't a priority for us right now. I agree with @carljm that this TODO feels unnecessary right now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal_string.md`:37 on 2024-12-03 03:12_

```suggestion
from typing import LiteralString

x: LiteralString

def f():
    reveal_type(x)  # revealed: LiteralString
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4650 on 2024-12-03 03:17_

Nice error message!

---

_@carljm approved on 2024-12-03 03:27_

Thanks!

---

_Merged by @carljm on 2024-12-03 03:31_

---

_Closed by @carljm on 2024-12-03 03:31_

---

_Branch deleted on 2024-12-04 18:58_

---

```yaml
number: 19382
title: "[ty] Expansion of enums into unions of literals"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/enum-expansion
created_at: 2025-07-16T09:56:13Z
updated_at: 2025-07-21T17:37:57Z
url: https://github.com/astral-sh/ruff/pull/19382
synced_at: 2026-01-12T15:56:38Z
```

# [ty] Expansion of enums into unions of literals

---

_@sharkdp_

## Summary

Implement expansion of enums into unions of enum literals (and the reverse operation). For the enum below, this allows us to understand that `Color = Literal[Color.RED, Color.GREEN, Color.BLUE]`, or that `Color & ~Literal[Color.RED] = Literal[Color.GREEN, Color.BLUE]`. This helps in exhaustiveness checking, which is why we see some removed `assert_never` false positives. And since exhaustiveness checking also helps with understanding terminal control flow, we also see a few removed `invalid-return-type` and `possibly-unresolved-reference` false positives. This PR also adds expansion of enums in overload resolution and type narrowing constructs.

```py
from enum import Enum
from typing_extensions import Literal, assert_never
from ty_extensions import Intersection, Not, static_assert, is_equivalent_to

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

type Red = Literal[Color.RED]
type Green = Literal[Color.GREEN]
type Blue = Literal[Color.BLUE]

static_assert(is_equivalent_to(Red | Green | Blue, Color))
static_assert(is_equivalent_to(Intersection[Color, Not[Red]], Green | Blue))


def color_name(color: Color) -> str:  # no error here (we detect that this can not implicitly return None)
    if color is Color.RED:
        return "Red"
    elif color is Color.GREEN:
        return "Green"
    elif color is Color.BLUE:
        return "Blue"
    else:
        assert_never(color)  # no error here
```

## Performance

I avoided an initial regression here for large enums, but the `UnionBuilder` and `IntersectionBuilder` parts can certainly still be optimized. We might want to use the same technique that we also use for unions of other literals. I didn't see any problems in our benchmarks so far, so this is not included yet.

## Test Plan

Many new Markdown tests

---

_Label `ty` added by @sharkdp on 2025-07-16 09:56_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-16 09:56_

---

_Comment by @github-actions[bot] on 2025-07-16 09:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-return-type] pwndbg/dbg/gdb/__init__.py:1660:10: Function can implicitly return `None`, which is not assignable to return type `((...) -> T, /) -> (...) -> T`
- Found 2270 diagnostics
+ Found 2269 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[invalid-return-type] src/urllib3/_collections.py:386:20: Return type does not match returned value: expected `list[str] | _DT`, found `(_Sentinel & ~Literal[_Sentinel.not_passed]) | (_DT & ~Literal[_Sentinel.not_passed])`
- error[invalid-argument-type] src/urllib3/util/connection.py:70:33: Argument to bound method `settimeout` is incorrect: Expected `int | float | None`, found `Unknown | _TYPE_DEFAULT`
- Found 402 diagnostics
+ Found 400 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] src/_pytest/fixtures.py:157:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Scope & ~Literal[Scope.Function] & ~Literal[Scope.Class] & ~Literal[Scope.Module] & ~Literal[Scope.Package] & ~Literal[Scope.Session]`
- error[invalid-argument-type] src/_pytest/fixtures.py:218:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Scope & ~Literal[Scope.Function] & ~Literal[Scope.Session] & ~Literal[Scope.Package] & ~Literal[Scope.Module] & ~Literal[Scope.Class]`
- error[invalid-argument-type] src/_pytest/pathlib.py:585:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `ImportMode & ~Literal[ImportMode.importlib] & ~Literal[ImportMode.append] & ~Literal[ImportMode.prepend]`
- error[invalid-argument-type] testing/test_cacheprovider.py:1303:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Action & ~Literal[Action.MKDIR] & ~Literal[Action.SET]`
- error[invalid-argument-type] testing/test_cacheprovider.py:1315:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Action & ~Literal[Action.MKDIR] & ~Literal[Action.SET]`
- Found 517 diagnostics
+ Found 512 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[possibly-unresolved-reference] static_frame/core/join.py:328:15: Name `final_index` used when possibly not defined
- Found 1792 diagnostics
+ Found 1791 diagnostics

meson (https://github.com/mesonbuild/meson)
- warning[possibly-unresolved-reference] mesonbuild/mlog.py:287:16: Name `label` used when possibly not defined
- Found 912 diagnostics
+ Found 911 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-16 10:04_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 6 | 0 |
| `invalid-return-type` | 0 | 2 | 0 |
| `possibly-unresolved-reference` | 0 | 2 | 0 |
| **Total** | **0** | **10** | **0** |

**[Full report with detailed diff](https://david-enum-expansion.ecosystem-663.pages.dev/diff)**


---

_Comment by @codspeed-hq[bot] on 2025-07-16 10:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fenum-expansion?runnerMode=Instrumentation)

### Merging #19382 will **not alter performance**

<sub>Comparing <code>david/enum-expansion</code> (1252ef8) with <code>main</code> (5cace28)</sub>



### Summary

`✅ 41` untouched benchmarks  





---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-17 08:59_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-17 08:59_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-17 10:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-17 10:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-17 12:48_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-17 12:48_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-17 13:00_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-17 13:00_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-18 11:58_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-18 11:59_

---

_Marked ready for review by @sharkdp on 2025-07-18 12:02_

---

_Review requested from @carljm by @sharkdp on 2025-07-18 12:02_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-18 12:02_

---

_Review requested from @dcreager by @sharkdp on 2025-07-18 12:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md`:49 on 2025-07-19 00:34_

I think we need to forbid custom `__eq__` on enum types in order to make this sound. Or maybe mark enum types with custom `__eq__` and refuse to do equality narrowing on them? (I'm fine with this just being an issue for later follow-up, if anyone ever cares enough.)

```
>>> class Color(Enum):
...     RED = 1
...     BLUE = 2
...     def __eq__(self, other):
...         return True
...
>>> Color.RED == Color.BLUE
True
>>> Color.RED == 2
True
>>> Color.RED == 7
True
```

---

_@carljm reviewed on 2025-07-19 00:39_

Only got to reviewing the tests so far, but the tests look great!

---

_@sharkdp reviewed on 2025-07-19 18:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md`:49 on 2025-07-19 18:49_

This is already implemented, and tested in the `is_single_valued` tests (see `ComparesEqualEnum`). I already felt like I was adding some tests in too many places, so I wasn't sure if it should be added here as well. But happy to do so.

Here's how it works:

```py
class Color(Enum):
    RED = 1
    BLUE = 2

def _(c: Color):
    if c == Color.RED:
        reveal_type(c)  # Literal[Color.RED]
```

vs

```py
class Color(Enum):
    RED = 1
    BLUE = 2

    def __eq__(self, other):
        return True


def _(c: Color):
    if c == Color.RED:
        reveal_type(c)  # Color
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1442 on 2025-07-21 13:00_

this feels like slightly less indirection (probably won't make much of a difference for performance, but maybe slightly more readable?)

```suggestion
                is_single_member_enum(db, target_enum_literal.enum_class(db))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:1 on 2025-07-21 13:04_

you could also add a test for differently ordered unions here -- e.g. it wasn't immediately obvious to me that something like this would pass with your implementation (it's awesome that it does!)

```py
from enum import Enum
from ty_extensions import static_assert
from typing import Literal

class A: ...
class B: ...

class MyEnum(Enum):
    SINGLE = object()

static_assert(is_equivalent_to(A | B | MyEnum, Literal[MyEnum.SINGLE] | B | A))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:383 on 2025-07-21 13:07_

nit: a union has members as well as an enum; I'd find this name slightly clearer:

```suggestion
                let enum_members_in_union = self
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:378 on 2025-07-21 13:11_

nit: there's a lot of literals floating around in this branch. Something like this name might be a bit clearer?

```suggestion
            Type::EnumLiteral(enum_member_to_add) => {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:392 on 2025-07-21 13:22_

I'd be tempted to do something like this:

```diff
diff --git a/crates/ty_python_semantic/src/types/builder.rs b/crates/ty_python_semantic/src/types/builder.rs
index c3680b7a99..dc10fe9c1c 100644
--- a/crates/ty_python_semantic/src/types/builder.rs
+++ b/crates/ty_python_semantic/src/types/builder.rs
@@ -88,6 +88,13 @@ enum UnionElement<'db> {
 }
 
 impl<'db> UnionElement<'db> {
+    const fn to_type_element(&self) -> Option<Type<'db>> {
+        match self {
+            UnionElement::Type(ty) => Some(*ty),
+            _ => None,
+        }
+    }
+
     /// Try reducing this `UnionElement` given the presence in the same union of `other_type`.
     fn try_reduce(&mut self, db: &'db dyn Db, other_type: Type<'db>) -> ReduceResult<'db> {
         match self {
@@ -383,13 +390,9 @@ impl<'db> UnionBuilder<'db> {
                 let members_in_union = self
                     .elements
                     .iter()
-                    .filter_map(|element| {
-                        if let UnionElement::Type(Type::EnumLiteral(lit)) = element {
-                            Some(lit.name(self.db).clone())
-                        } else {
-                            None
-                        }
-                    })
+                    .filter_map(UnionElement::to_type_element)
+                    .filter_map(Type::into_enum_literal)
+                    .map(|literal| literal.name(self.db).clone())
                     .chain(std::iter::once(enum_literal.name(self.db).clone()))
                     .collect::<FxOrderSet<_>>();
 
@@ -401,16 +404,14 @@ impl<'db> UnionBuilder<'db> {
 
                 if all_members_are_in_union {
                     self.add_in_place(enum_literal.enum_class_instance(self.db));
-                } else {
-                    if !self.elements.iter().any(|element| match element {
-                        UnionElement::Type(ty) => {
-                            Type::EnumLiteral(enum_literal).is_subtype_of(self.db, *ty)
-                        }
-                        _ => false,
-                    }) {
-                        self.elements
-                            .push(UnionElement::Type(Type::EnumLiteral(enum_literal)));
-                    }
+                } else if !self
+                    .elements
+                    .iter()
+                    .filter_map(UnionElement::to_type_element)
+                    .any(|ty| Type::EnumLiteral(enum_literal).is_subtype_of(self.db, ty))
+                {
+                    self.elements
+                        .push(UnionElement::Type(Type::EnumLiteral(enum_literal)));
                 }
             }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:600 on 2025-07-21 13:23_

oh, that's clever!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:626 on 2025-07-21 13:25_

```suggestion
            self.intersections
                .iter()
                .flat_map(|intersection| &intersection.positive)
                .any(|ty| *ty == enum_instance)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/arguments.rs`:216 on 2025-07-21 13:30_

it looks like `enum_member_literals()` itself calls `enum_metadata()` internally, so it seems a bit duplicative to have to call `enum_metadata` first here. Should `enum_member_literals` return `None` rather than returning an empty iterator if it's called on a non-enum class (or an enum class that doesn't have any members)?

Also, do we have any tests for overloads that involve `Enum` subclasses without any members? I'm guessing we shouldn't do any overload expansion of any kind for those classes; it might be good to have a test explicitly asserting that, if we don't already

---

_@AlexWaygood approved on 2025-07-21 13:33_

---

_@sharkdp reviewed on 2025-07-21 14:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/builder.rs`:392 on 2025-07-21 14:01_

Love it.

---

_@sharkdp reviewed on 2025-07-21 14:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/builder.rs`:600 on 2025-07-21 14:03_

Initially, I duplicated what the `Type::Union` branch does here, but wanted to get rid of that. Fortunately, the wrapping/unwrapping in `UnionType` doesn't appear to hurt, performance-wise.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/arguments.rs`:216 on 2025-07-21 14:36_

> it looks like `enum_member_literals()` itself calls `enum_metadata()` internally, so it seems a bit duplicative to have to call `enum_metadata` first here. Should `enum_member_literals` return `None` rather than returning an empty iterator if it's called on a non-enum class (or an enum class that doesn't have any members)?

Good suggestion — changed!



> Also, do we have any tests for overloads that involve `Enum` subclasses without any members? I'm guessing we shouldn't do any overload expansion of any kind for those classes; it might be good to have a test explicitly asserting that, if we don't already

Added

---

_@sharkdp reviewed on 2025-07-21 14:36_

---

_Merged by @sharkdp on 2025-07-21 17:37_

---

_Closed by @sharkdp on 2025-07-21 17:37_

---

_Branch deleted on 2025-07-21 17:37_

---

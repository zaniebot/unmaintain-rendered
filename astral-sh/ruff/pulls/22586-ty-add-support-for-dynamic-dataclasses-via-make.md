```yaml
number: 22586
title: "[ty] Add support for dynamic dataclasses via `make_dataclass`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: charlie/functional-dict
created_at: 2026-01-14T22:20:58Z
updated_at: 2026-01-22T02:53:56Z
url: https://github.com/astral-sh/ruff/pull/22586
synced_at: 2026-01-22T03:09:27Z
```

# [ty] Add support for dynamic dataclasses via `make_dataclass`

---

_@charliermarsh_

## Summary

Like #22327, but for dataclasses.


---

_Label `ty` added by @charliermarsh on 2026-01-14 22:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | dict[str, Any] | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/experimental/pydantic/error_type.py:149:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/experimental/pydantic/object_type.py:255:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 345 diagnostics
+ Found 347 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @carljm by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-15 02:58_

---

_Converted to draft by @charliermarsh on 2026-01-15 02:59_

---

_Marked ready for review by @charliermarsh on 2026-01-15 03:22_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-15 03:59_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-15 09:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-await` | 0 | 0 | 6 |
| `unused-ignore-comment` | 4 | 0 | 0 |
| `invalid-argument-type` | 1 | 2 | 0 |
| **Total** | **5** | **2** | **6** |


**[Full report with detailed diff](https://50588323.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://50588323.ty-ecosystem-ext.pages.dev/timing))



---

_Closed by @AlexWaygood on 2026-01-15 13:33_

---

_Reopened by @AlexWaygood on 2026-01-15 13:34_

---

_Comment by @AlexWaygood on 2026-01-15 13:46_

I'm a _bit_ hesitant about this one. The typing spec doesn't say anything about this function AFAIK, and I don't remember seeing any user requests on the issue tracker for us to add special-cased support. Mypy_primer only shows two diagnostics going away in the ecosystem, on `strawberry` -- and it looks like those lines already have `type: ignore`s on them for mypy.

Meanwhile, this is _quite_ a bit of new code, and the manual parsing of arguments in `infer/builder.rs` is quite complicated and error-prone.

Do any other type checkers have special-cased support for this function? We do have to draw the line _somewhere_.

I think I'd feel differently if we could share more of the call-expression-parsing logic with what we've already added for namedtuples (which definitely _was_ necessary), but the schema `make_dataclass()` expects is _just_ different enough that sharing more of the code seems like it could be pretty difficult?

Another -- much simpler -- way to avoid false positives with the objects returned by this function would be to just special-case the return type so that we infer `type[Unknown]` rather than `type` -- that wouldn't involve any custom call-expression parsing.

---

_Comment by @charliermarsh on 2026-01-15 13:59_

AFAICT, Mypy and Pyright do not support it. But... I would still advocate for us to support it? We have all the infrastructure to do so! And almost all of the new code is in argument parsing -- which is very verbose, but at least fairly mechanical and testable? (E.g., it's much easier to know when we have that right, as opposed to something deep in type inference.) I think I don't agree that _this_ is where we should draw the line, unless it's a feature that we can't support for reasons that I don't yet understand or would be uncovered by your review.


---

_Comment by @AlexWaygood on 2026-01-15 14:01_

(I'm curious for @carljm's and @dcreager's opinions!)

---

_Comment by @charliermarsh on 2026-01-15 14:08_

Ditto and am totally fine to be overruled on it of course. I took it as a given that we'd support this given that it's in the [type system overview](https://github.com/astral-sh/ty/issues/1889); that may have been a mistake!

---

_Comment by @AlexWaygood on 2026-01-15 14:10_

> Ditto and am totally fine to be overruled on it of course. I took it as a given that we'd support this given that it's in the [type system overview](https://github.com/astral-sh/ty/issues/1889); that may have been a mistake!

Well, that issue just says that we don't have any tests for the function right now. But this PR adds a lot more than just test coverage 😆

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:7836 on 2026-01-15 14:12_

Can we match on the elements here. The same in other positions
```suggestion
                    [name_expr, type_expr] => {
```

---

_@MichaReiser reviewed on 2026-01-15 14:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:6557 on 2026-01-15 14:13_

It seems unfortunate that we have to repeat all those methods for every dynamic class literal. Can't we share more infrastructure?

---

_@MichaReiser reviewed on 2026-01-15 14:13_

---

_Comment by @carljm on 2026-01-16 00:53_

I think it's pretty natural to support this (along with similar forms for namedtuple and TypedDict) and it seems pretty easy to do. I'm not really concerned by the argument parsing. It's a chunk of code, sure, but it's not a chunk of code that interacts with other things in a way that multiplies complexity; it's pretty much standalone, we'll only ever touch it if literally `make_dataclass` support is broken, and then we'll know right where to look.

The fact that other type checkers don't support it means I wouldn't have considered it a priority for stable -- but if it's done and working, I don't see any significant downside to merging it.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:56 on 2026-01-16 11:44_

```suggestion
from dataclasses import make_dataclass
from ty_extensions import reveal_mro

Point2 = make_dataclass("Point2", [("x", int), ("y", int)])

reveal_type(Point2)  # revealed: <class 'Point2'>
reveal_mro(Point2)  # revealed: (<class 'Point2'>, <class 'object'>)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:76 on 2026-01-16 11:44_

nice!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:108 on 2026-01-16 11:45_

I'm not sure this test shows that much as-is, because all Python types inherit an `__eq__` method from `object` even if they don't define `__eq__`, so you should be able to compare nearly all Python types using `==` without it failing

---

_Comment by @AlexWaygood on 2026-01-16 11:49_

Okiedokie

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:183 on 2026-01-16 11:51_

Can we also test that we view these attributes as immutable? (I can't remember exactly what error we should be emitting here)

```suggestion
from dataclasses import make_dataclass

PointFrozen = make_dataclass("PointFrozen", [("x", int), ("y", int)], frozen=True)

p = PointFrozen(1, 2)

# frozen dataclasses generate __hash__
reveal_type(hash(p))  # revealed: int

# These should both fail, since frozen dataclasses are immutable
p.x = 42  # error
p.y = 56  # error
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:262 on 2026-01-16 11:53_

```suggestion
from dataclasses import make_dataclass
from ty_extensions import reveal_mro

class Base:
    def greet(self) -> str:
        return "Hello"

Derived = make_dataclass("Derived", [("value", int)], bases=(Base,))
reveal_mro(Derived)  # revealed: (<class 'Derived'>, <class 'Base'>, <class 'object'>)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:294 on 2026-01-16 11:57_

```suggestion
from dataclasses import make_dataclass
from ty_extensions import reveal_mro

def get_fields():
    return [("x", int)]

fields = get_fields()
PointDynamic = make_dataclass("PointDynamic", fields)

p = PointDynamic(1)  # No error - accepts any arguments
reveal_type(p.x)  # revealed: Any

# the class is still inferred as inheriting directly from `object`
# (`Unknown` is not inserted into the MRO)
reveal_mro(PointDynamic)  # revealed: (<class 'PointDynamic'>, <class 'object'>)

# ...but nontheless, we assume that all attributes are available,
# similar to attribute access on `Unknown`
reveal_type(p.unknown)  # revealed: Any
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:331 on 2026-01-16 11:58_

But you don't currently emit a diagnostic on this branch for too _few_ positional arguments -- this does not cause us to emit an error on this branch, but it fails at runtime:

```py
from dataclasses import make_dataclass

make_dataclass("foo")
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:428 on 2026-01-16 12:00_

You don't currently emit an error for `make_dataclass("foo", bases=12345)` on this branch

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:343 on 2026-01-16 12:02_

I think we'd be able to infer the MRO just fine, but `Protocol` classes are heavily special-cased elsewhere, and it would significantly complicate our internal logic to support dynamically constructed `Protocol` classes (which anyway aren't mentioned anywhere by the typing spec)

```suggestion
Protocol bases use a different lint (`unsupported-dynamic-base`) because they're technically valid
Python but not supported by ty for dynamic classes.
```

---

_@AlexWaygood reviewed on 2026-01-16 12:02_

A tests-only review for now

---

_Converted to draft by @charliermarsh on 2026-01-16 13:40_

---

_Marked ready for review by @charliermarsh on 2026-01-16 14:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/make_dataclass.md`:1 on 2026-01-16 16:23_

Not sure you have any tests yet for what happens if `*args` or `**kwargs` are passed into a `make_dataclass` call.

Also (sorry): you should probably add tests for recursive and stringified types, e.g.

```py
from dataclasses import make_dataclass

X = make_dataclass("X", [("a", "X | None")])
```

Since we'll _have_ to support recursive and stringified types for functional `typing.NamedTuple`s and functional `typing.TypedDict`s, we should probably be consistent and support them for functional dataclasses too.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:676 on 2026-01-16 16:25_

do we have tests for what happens if functional dataclasses with and without `order=True` are passed to `total_ordering()`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:712 on 2026-01-16 16:29_

I think you could reduce duplication between the branches a little bit in this method, e.g.

```rs
    /// Returns the type definition for this class.
    ///
    /// For static classes, returns `TypeDefinition::StaticClass`.
    /// For dynamic classes, returns `TypeDefinition::DynamicClass` if a definition is available.
    pub(crate) fn type_definition(self, db: &'db dyn Db) -> Option<TypeDefinition<'db>> {
        let definition = match self {
            Self::Dynamic(class) => class.definition(db),
            Self::DynamicNamedTuple(namedtuple) => namedtuple.definition(db),
            Self::DynamicDataclass(dataclass) => dataclass.definition(db),

            // This is the only variant that returns `TypeDefinition::StaticClass`
            // rather than `TypeDefinition::DynamicClass`.
            Self::Static(class) => return Some(TypeDefinition::StaticClass(class.definition(db))),
        };

        definition.map(TypeDefinition::DynamicClass)
    }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:771 on 2026-01-16 16:30_

do we have a test that demonstrates that a `slots=True` functional dataclass is understood as a disjoint base?

---

_@AlexWaygood reviewed on 2026-01-16 16:31_

---

_Comment by @charliermarsh on 2026-01-16 20:00_

Can probably hold off on further review until https://github.com/astral-sh/ruff/pull/22627 is resolved, since it will have implications for this PR.

---

_Converted to draft by @charliermarsh on 2026-01-19 13:41_

---

_Comment by @charliermarsh on 2026-01-19 13:41_

(Marking as draft while we finish the recursive definition work upstream.)

---

_Review request for @carljm removed by @carljm on 2026-01-21 23:47_

---

```yaml
number: 19850
title: "[ty] print diagnostics with fully qualified name to disambiguate some cases"
type: pull_request
state: merged
author: leandrobbraga
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: issue-848
created_at: 2025-08-10T21:36:15Z
updated_at: 2025-08-27T20:46:13Z
url: https://github.com/astral-sh/ruff/pull/19850
synced_at: 2026-01-10T17:46:21Z
```

# [ty] print diagnostics with fully qualified name to disambiguate some cases

---

_Pull request opened by @leandrobbraga on 2025-08-10 21:36_

There are some situations that we have a confusing diagnostics due to identical class names.

## Class with same name from different modules

```python
import pandas
import polars

df: pandas.DataFrame = polars.DataFrame()
```

This yields the following error:

**Actual:**
error: [invalid-assignment] "Object of type `DataFrame` is not assignable to `DataFrame`"
**Expected**:
error: [invalid-assignment] "Object of type `polars.DataFrame` is not assignable to `pandas.DataFrame`"

## Nested classes

```python
from enum import Enum

class A:
    class B(Enum):
        ACTIVE = "active"
        INACTIVE = "inactive"

class C:
    class B(Enum):
        ACTIVE = "active"
        INACTIVE = "inactive"
```

**Actual**:
error: [invalid-assignment] "Object of type `Literal[B.ACTIVE]` is not assignable to `B`"
**Expected**:
error: [invalid-assignment] "Object of type `Literal[my_module.C.B.ACTIVE]` is not assignable to `my_module.A.B`"

## Solution

In this MR we added an heuristics to detect when to use a fully qualified name:
- There is an invalid assignment and;
- They are two different classes and;
- They have the same name

The fully qualified name always includes:
- module name
- nested classes name
- actual class name

There was no `QualifiedDisplay` so I had to implement it from scratch. I'm very new to the codebase, so I might have done things inefficiently, so I appreciate feedback. 

Should we pre-compute the fully qualified name or do it on demand? 

## Not implemented

### Function-local classes

Should we approach this in a different PR?

**Example**:
```python 
# t.py
from __future__ import annotations


def function() -> A:
    class A:
        pass

    return A()


class A:
    pass


a: A = function()
```

#### mypy

```console
t.py:8: error: Incompatible return value type (got "t.A@5", expected "t.A")  [return-value]
```

From my testing the 5 in `A@5` comes from the like number. 

#### ty

```console
error[invalid-return-type]: Return type does not match returned value
 --> t.py:4:19
  |
4 | def function() -> A:
  |                   - Expected `A` because of return type
5 |     class A:
6 |         pass
7 |
8 |     return A()
  |            ^^^ expected `A`, found `A`
  |
info: rule `invalid-return-type` is enabled by default
```

Fixes https://github.com/astral-sh/ty/issues/848

---

_Review requested from @carljm by @leandrobbraga on 2025-08-10 21:36_

---

_Review requested from @AlexWaygood by @leandrobbraga on 2025-08-10 21:36_

---

_Review requested from @sharkdp by @leandrobbraga on 2025-08-10 21:36_

---

_Review requested from @dcreager by @leandrobbraga on 2025-08-10 21:36_

---

_Comment by @github-actions[bot] on 2025-08-10 21:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-10 21:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @leandrobbraga on 2025-08-10 22:14_

It seems there are some false positives, I'll work on those later.

EDIT: it was an important check that I accidentally deleted while refactoring 

---

_Label `ty` added by @AlexWaygood on 2025-08-10 22:20_

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-10 22:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1880 on 2025-08-11 13:45_

There are a few more variants here where the class can pop up in the display:

```suggestion
        Type::NominalInstance(instance) => type_to_class_literal(Type::from(instance.class)),
        Type::EnumLiteral(enum_literal) => Some(enum_literal.enum_class(db)),
        Type::GenericAlias(alias) => Some(alias.origin(db)),
        Type::ProtocolInstance(ProtocolInstanceType { inner: Protocol::FromClass(class) }) => Some(class),
        Type::TypedDictType(typed_dict) => Some(typed_dict.defining_class(db)),
        Type::SubclassOf(subclass_of) => type_to_class_literal(Type::from(subclass_of.subclass_of().into_class()?)),
        _ => None,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:76 on 2025-08-11 13:46_

I'd also add branches for `GenericAlias`, `ProtocolInstance`, `SubclassOf` and `TypedDictType` here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:78 on 2025-08-11 13:48_

this can be slightly more performant than using the macro (though I'd still use the macro whenever it makes the code cleaner, e.g. in your `EnumLiteral` branch below)

```suggestion
                f.write_str(&self.get_qualified_representation(*literal))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:82 on 2025-08-11 13:48_

```suggestion
                    f.write_str(&self.get_qualified_representation(class))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:89 on 2025-08-11 13:49_

```suggestion
                    f.write_str(
                        &self.get_qualified_representation(alias.origin(self.db))
                    )
```

---

_@AlexWaygood reviewed on 2025-08-11 13:50_

Thank you! A few quick things I noticed:

---

_Comment by @leandrobbraga on 2025-08-12 01:23_

I had to rebase to solve conflicts, but I think I addressed all the suggestions.

I tried doing some testing to the `<locals of function 'f'>` but I couldn't come up with a working test for this situation.

---

_Review comment by @leandrobbraga on `crates/ty_python_semantic/src/types/display.rs`:76 on 2025-08-12 01:26_

Was this that you had in mind?

---

_@leandrobbraga reviewed on 2025-08-12 01:26_

---

_Review requested from @AlexWaygood by @leandrobbraga on 2025-08-13 16:17_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:184 on 2025-08-19 22:13_

Can we also add some tests similar to these where the name that needs qualifying is a nested type, e.g. an assignment fails between `list[mod1.A]` and `list[mod2.A]`. (It's OK if these tests remain TODO in this PR, but I want to record the fact that we still need to do this.)

This should be easy enough to support in the display implementation (especially if we refactor it as I've suggested); it will be a bit harder to detect when types need qualifying. It will probably require a more involved implementation of `are_these_types_ambiguously_named` which we call after knowing that we need to emit an assignment error, which actually descends into generic contexts when the outer types are matching generic classes. (Rather than just extracting the two top-level class types as the current implementation does.)

(There might be another possible approach where `has_relation_to` actually tracks the inner type mismatch and returns this information. It's _possible_ this would mesh nicely with https://github.com/astral-sh/ruff/pull/19838, but I don't think we should pay much extra cost to track information we only will need if emitting an error; it's usually better to do some extra work after we know there's an error. So I'd go with the above approach instead.)

---

_@carljm requested changes on 2025-08-19 22:14_

Thank you for working on this! 

Sorry for coming in on the late side with a new suggestion, but I think we should take a slightly different approach in the `display.rs` part of the implementation. Rather than introducing a totally separate `QualifiedDisplayType` struct, I think we should have a `qualified: bool` attribute on the existing `DisplayType` and `DisplayRepresentation structs, which we can respect whenever we are emitting the name of a class or function. (The current PR doesn't seem to handle functions, and it doesn't need to be done in this PR, but at some point I think we'll want to do this for function names, too.) I think this will make it much easier to share parts of the implementation that are not related to whether or not names are qualified, without code duplication.

The `qualified` flag could be set using a builder pattern, like `ty.display(db).qualified()` vs just `ty.display(db)` for the un-qualified display of a type. Though we may also want some form that allows easily passing a boolean value; this will be convenient for when we want to pass along the "qualified" value to display of a nested inner type.

The recently-merged implementation of a "multiline" display option (which probably needs conflict resolution with this PR anyway) is a good model to follow here.

---

_Review requested from @MichaReiser by @leandrobbraga on 2025-08-24 19:38_

---

_@leandrobbraga reviewed on 2025-08-24 19:46_

---

_Review comment by @leandrobbraga on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:184 on 2025-08-24 19:46_

I used the same approach as `multline` and it ended a much simpler design, I already pushed the commit to this PR.

---

_@leandrobbraga reviewed on 2025-08-24 21:56_

---

_Review comment by @leandrobbraga on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:184 on 2025-08-24 21:56_

I added a test such as: `dataframes: list[a.DataFrame] = [b.DataFrame()]` and left it as TODO like you mentioned, I can work on it on a different PR.

This assignment is not even failing, probably because of https://github.com/astral-sh/ty/issues/543

---

_Review requested from @carljm by @leandrobbraga on 2025-08-26 00:56_

---

_Review request for @sharkdp removed by @sharkdp on 2025-08-26 07:05_

---

_@carljm reviewed on 2025-08-27 19:45_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:184 on 2025-08-27 19:45_

Yes, that's why the assignment doesn't fail. I updated the test to use a function argument instead of an inferred literal container type to avoid this issue -- it's better anyway if the test is more independent from the details of container inference.

---

_@carljm reviewed on 2025-08-27 19:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1999 on 2025-08-27 19:57_

It would be great to have test coverage of all these match arms. I have Claude working on this right now, we'll see how it does.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:154 on 2025-08-27 19:59_

I don't think we have test coverage of this case. It would require trying to assign a class within the same function where it's defined, as there's no way to reference a class defined inside of a function outside that function. I'll see if Claude can add this next.

---

_@carljm reviewed on 2025-08-27 19:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:191 on 2025-08-27 20:02_

It seems like we do this in several places -- we should define a function for it.

---

_@carljm reviewed on 2025-08-27 20:04_

Looking good! I think we should increase our test coverage a bit, and try to reduce code boilerplate.

I think ultimately in order to support nested types we will probably need to change the settings from just having a `qualified` boolean, to instead having a vector of types whose names should be qualified, so that we can identify the right types to qualify even in nested display. But this can be a separate PR.

---

_@carljm approved on 2025-08-27 20:40_

I went ahead and made those updates, this looks good to go!

---

_Merged by @carljm on 2025-08-27 20:46_

---

_Closed by @carljm on 2025-08-27 20:46_

---

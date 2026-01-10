```yaml
number: 19874
title: "[ty] typecheck dict methods for `TypedDict`"
type: pull_request
state: merged
author: PrettyWood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ty-typeddict-methods
created_at: 2025-08-12T09:43:06Z
updated_at: 2025-08-29T14:25:04Z
url: https://github.com/astral-sh/ruff/pull/19874
synced_at: 2026-01-10T17:46:21Z
```

# [ty] typecheck dict methods for `TypedDict`

---

_Pull request opened by @PrettyWood on 2025-08-12 09:43_

## Summary

Typecheck `get()`, `setdefault()`, `pop()` for `TypedDict`

```py
from typing import TypedDict
from typing_extensions import NotRequired

class Employee(TypedDict):
    name: str
    department: NotRequired[str]

emp = Employee(name="Alice", department="Engineering")

emp.get("name")
emp.get("departmen", "Unknown")
emp.pop("department")
emp.pop("name")
```

<img width="838" height="529" alt="Screenshot 2025-08-12 at 11 42 12" src="https://github.com/user-attachments/assets/77ce150a-223c-4931-b914-551095d8a3a6" />


part of https://github.com/astral-sh/ty/issues/154

## Test Plan

Updated Markdown tests


---

_Review requested from @carljm by @PrettyWood on 2025-08-12 09:43_

---

_Review requested from @AlexWaygood by @PrettyWood on 2025-08-12 09:43_

---

_Review requested from @sharkdp by @PrettyWood on 2025-08-12 09:43_

---

_Review requested from @dcreager by @PrettyWood on 2025-08-12 09:43_

---

_Review requested from @MichaReiser by @PrettyWood on 2025-08-12 09:43_

---

_Label `ty` added by @AlexWaygood on 2025-08-12 10:58_

---

_Review request for @carljm removed by @carljm on 2025-08-19 00:23_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-25 12:51_

---

_Comment by @github-actions[bot] on 2025-08-25 12:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-29 13:40:04.333301009 +0000
+++ new-output.txt	2025-08-29 13:40:06.963314808 +0000
@@ -849,7 +849,6 @@
 tuples_type_form.py:25:1: error[invalid-assignment] Object of type `tuple[Literal[1]]` is not assignable to `tuple[()]`
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:37:5: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
-typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
 typeddicts_readonly_inheritance.py:65:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
@@ -859,5 +858,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 860 diagnostics
+Found 859 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-25 12:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/code.py:447:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[str] | None`
+ openlibrary/plugins/worksearch/code.py:447:61: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[str] | None`
- Found 709 diagnostics
+ Found 711 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/activity.py:882:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/member.py:390:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 533 diagnostics
+ Found 531 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/interpreter.py:1804:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | None`, found `dict[OptionKey, str | int | list[str]]`
+ mesonbuild/modules/python.py:169:42: error[invalid-key] Invalid key access on TypedDict `ExtensionModuleKw`: Unknown key "limited_api"
- Found 808 diagnostics
+ Found 810 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/runner/storage.py:851:13: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | (Any & ~AlwaysFalsy & ~Secret[Unknown]) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
+ src/prefect/runner/storage.py:858:13: error[invalid-return-type] Return type does not match returned value: expected `str`, found `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | str | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
+ src/prefect/runner/storage.py:859:16: warning[possibly-unbound-attribute] Attribute `startswith` on type `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | (str & ~AlwaysFalsy) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)` is possibly unbound
+ src/prefect/runner/storage.py:866:13: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | (Any & ~AlwaysFalsy & ~Secret[Unknown]) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
+ src/prefect/runner/storage.py:867:20: warning[possibly-unbound-attribute] Attribute `startswith` on type `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | (str & ~AlwaysFalsy) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)` is possibly unbound
+ src/prefect/runner/storage.py:872:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | (str & ~AlwaysFalsy) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
- Found 3013 diagnostics
+ Found 3019 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/llmobs/_experiment.py:506:28: warning[redundant-cast] Value is already of type `int`
- Found 6602 diagnostics
+ Found 6603 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2331 on 2025-08-25 12:56_

I think that would be more of an opinionated lint rule, rather than a hard error. So I suggest we remove that comment for now:
```suggestion
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2388 on 2025-08-25 12:59_

If the return type is the same (and should stay the same, see my other comment), could we merge these two overloads (with and without default value) into a single overload where the `default` parameter has a default value? 

---

_Comment by @github-actions[bot] on 2025-08-25 13:01_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 4 | 0 | 0 |
| `invalid-argument-type` | 3 | 0 | 0 |
| `possibly-unbound-attribute` | 2 | 0 | 0 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| `invalid-key` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **11** | **2** | **0** |


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2339 on 2025-08-25 13:19_

I'm wondering if this signature is too strict? Take this example:
```py
from typing import TypedDict, reveal_type

class T(TypedDict, total=False):
    a: str

def _(t: T):
    reveal_type(t.get("a", 0))
```

mypy, pyright, and pyrefly all agree that this code is fine and that the type of the `.get` call should be `str | Literal[0]` (or `str | int`). Instead, with this synthesized overload here, we get an error and a return type of `Unknown`:

```
info[revealed-type]: Revealed type
 --> /home/shark/playground/test.py:9:17
  |
8 | def _(t: T):
9 |     reveal_type(t.get("a", 0))
  |                 ^^^^^^^^^^^^^ `Unknown`
  |

error[invalid-argument-type]: Argument is incorrect
 --> /home/shark/playground/test.py:9:28
  |
8 | def _(t: T):
9 |     reveal_type(t.get("a", 0))
  |                            ^ Expected `str`, found `Literal[0]`
  |
```

I have not checked if this behavior is specified somewhere (we should do that), but if not, it feels safer to go with the behavior or the other type checkers here and return a union of the declared field type and the type of the default argument. This would probably require us to make that overload generic.


A similar comment might also apply to `.pop` and `.setdefault`?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6277 on 2025-08-25 13:26_

This won't capture all `.pop` calls (imagine e.g. `method = person.pop; method("some_required_key")`, but it's still a very nice touch!

It's not impossible to make this work for all calls, I think. But it would require a significant rework of our call machinery, and I don't think it's important enough to do.

Even if we miss some calls to these methods by explicitly matching on `Expr::Attribute`, it's not like we won't emit an error. We will only fall back to a worse error message (e.g. no-matching-overload), which seems fine to me if it's relatively rare. I think this behavior might be worth documenting in a test, though.

---

_@sharkdp requested changes on 2025-08-25 13:28_

Thank you very much. This looks great.

I looked through a few ecosystem changes here (and found a problem with `.get` calls with a default argument of a different type, which I commented on inline), but it might be worth going through a few more to see if we're missing anything in the implementation.

---

_Comment by @sharkdp on 2025-08-29 11:06_

I'm currently looking into making the required changes here.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-29 11:54_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-29 11:54_

---

_@sharkdp reviewed on 2025-08-29 11:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:7552 on 2025-08-29 11:57_

@dcreager Would be great if you could take a short look at this new method here.. and the new constructor in `generics.rs`. Mainly to check my wording in the doc comments / function names.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-29 12:10_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-29 12:10_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-29 12:55_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-29 12:55_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2419 on 2025-08-29 13:06_

nit: `CallableSignature::from_overloads` takes in an iterator, so I think you can skip the `collect` here

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2474 on 2025-08-29 13:06_

and here

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2503 on 2025-08-29 13:06_

and here

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:131 on 2025-08-29 13:10_

nit: I'd suggest removing the "(synthetic)" part, since that doesn't have to be a limitation on when you can use this constructor.  And I think you could simplify `from_type_params` and `normalized_impl` to delegate to this constructor to do the collecting.

---

_@dcreager reviewed on 2025-08-29 13:10_

Just a couple of comments re the generics/typevar changes.

---

_Comment by @sharkdp on 2025-08-29 13:19_

Ok, after adding some fallback overloads for `.get`, we're now down to a reasonable number of ecosystem changes. 

```diff
discord.py (https://github.com/Rapptz/discord.py)
- discord/activity.py:882:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/member.py:390:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 533 diagnostics
+ Found 531 diagnostics
```

Correct, they are `.pop` ing a required key, and silence that with `# type: ignore`.

```diff
openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/code.py:447:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[str] | None`
+ openlibrary/plugins/worksearch/code.py:447:61: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[str] | None`
- Found 709 diagnostics
+ Found 711 diagnostics
```

True positives. They have a key with value type `list[str] | None`, and then use `.get("that_key", [])` in an attempt to replace `None` with `[]`. But that doesn't work. `get` won't replace values of type `None` with the default. It only replaces missing keys with the default.

```diff
meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/interpreter.py:1804:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | None`, found `dict[OptionKey, str | int | list[str]]`
+ mesonbuild/modules/python.py:169:42: error[invalid-key] Invalid key access on TypedDict `ExtensionModuleKw`: Unknown key "limited_api"
- Found 808 diagnostics
+ Found 810 diagnostics
```

Both look like true positives to me

```diff
prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/runner/storage.py:851:13: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | (Any & ~AlwaysFalsy & ~Secret[Unknown]) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
+ src/prefect/runner/storage.py:858:13: error[invalid-return-type] Return type does not match returned value: expected `str`, found `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | str | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
+ src/prefect/runner/storage.py:859:16: warning[possibly-unbound-attribute] Attribute `startswith` on type `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | (str & ~AlwaysFalsy) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)` is possibly unbound
+ src/prefect/runner/storage.py:866:13: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | (Any & ~AlwaysFalsy & ~Secret[Unknown]) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
+ src/prefect/runner/storage.py:867:20: warning[possibly-unbound-attribute] Attribute `startswith` on type `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | (str & ~AlwaysFalsy) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)` is possibly unbound
+ src/prefect/runner/storage.py:872:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `(Any & ~AlwaysFalsy & ~Secret[Unknown]) | (str & ~AlwaysFalsy) | (Secret[str] & ~AlwaysFalsy & ~Secret[Unknown]) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy)`
- Found 3013 diagnostics
+ Found 3019 diagnostics
```

Seems unrelated to `TypedDict`, only surfaces because we don't infer `Unknown` anymore. Potentially related to https://github.com/astral-sh/ty/issues/456.

```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/llmobs/_experiment.py:506:28: warning[redundant-cast] Value is already of type `int`
- Found 6602 diagnostics
+ Found 6603 diagnostics
```

True positive

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2419 on 2025-08-29 13:23_

Oops, yes. I missed this in my review. Thanks.

---

_@sharkdp reviewed on 2025-08-29 13:23_

---

_@sharkdp approved on 2025-08-29 13:30_

Thanks again!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2396 on 2025-08-29 13:33_

Wouldn't `Unknown | None` be better here, to express the fact that the unknown key might not be present (it might be a subtype of this typeddict, but it also might not?)?

```suggestion
                            Some(UnionType::from_elements(db, [Type::unknown(), Type::none(db)])),
```

---

_@AlexWaygood reviewed on 2025-08-29 13:33_

---

_@sharkdp reviewed on 2025-08-29 13:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2396 on 2025-08-29 13:39_

Yes, that makes sense — changed. Let's see if that leads to new diagnostics.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:487 on 2025-08-29 13:42_

I feel like ideally we'd still infer `str` here (despite the fact that the operation is illegal, it seems pretty clear that the result of this operation will always be a `str`), but I can see that that might be difficult to do _consistently_ with the synthesized-overload approach. And consistently inferring `Unknown` here seems better than sometimes inferring `Unknown`, sometimes `str` (depending on exactly how the method is invoked on the typeddict object).

---

_@AlexWaygood approved on 2025-08-29 13:42_

---

_@sharkdp reviewed on 2025-08-29 14:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:487 on 2025-08-29 14:24_

> I feel like ideally we'd still infer `str` here (despite the fact that the operation is illegal, it seems pretty clear that the result of this operation will always be a `str`)

Yes, interesting point. I agree that this would probably be the better return type, but as you said, it's not possible to achieve that with synthesized overloads, unless we manage to synthesize overloads that trigger specific `CallError`s when selected, or similar. Until then, `Unknown` is not ideal, but it's also not wrong. And if you're going to `# type: ignore` your illegal `.pop` operation, you might as well throw in a `cast` or a redeclaration :-)

---

_Merged by @sharkdp on 2025-08-29 14:25_

---

_Closed by @sharkdp on 2025-08-29 14:25_

---

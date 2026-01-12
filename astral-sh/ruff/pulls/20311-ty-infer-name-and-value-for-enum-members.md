```yaml
number: 20311
title: "[ty] infer `name` and `value` for enum members"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: thejchap/enum-members
created_at: 2025-09-09T02:49:46Z
updated_at: 2025-09-17T07:36:27Z
url: https://github.com/astral-sh/ruff/pull/20311
synced_at: 2026-01-12T15:56:58Z
```

# [ty] infer `name` and `value` for enum members

---

_@thejchap_

## summary
- this pr implements the following attributes for `Enum` members:
  - `name`
  - `_name_`
  - `value`
  - `_value_`
- adds a TODO test for `my_enum_class_instance.name`
- only implements if the instance is a subclass of `Enum` re: this [comment](https://github.com/astral-sh/ruff/pull/19481#issuecomment-3103460307) and existing [test](https://github.com/thejchap/ruff/blob/c34449ed7ca4b043d514240e212335a2554d0fb1/crates/ty_python_semantic/resources/mdtest/enums.md?plain=1#L625)

### pointers
- https://github.com/astral-sh/ty/issues/876
- https://typing.python.org/en/latest/spec/enums.html#enum-definition
- https://github.com/astral-sh/ruff/pull/19481#issuecomment-3103460307

## test plan
- mdtests
- triaged conformance diffs here: https://diffswarm.dev/d-01k531ag4nee3xmdeq4f3j66pb
- triaged mypy primer diffs here for django-stubs: https://diffswarm.dev/d-01k5331n13k9yx8tvnxnkeawp3
  - added a TODO test for overriding `.value`
- discord diff seems reasonable

---

_Label `ty` added by @AlexWaygood on 2025-09-09 07:20_

---

_Comment by @github-actions[bot] on 2025-09-10 01:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-17 07:23:56.516264569 +0000
+++ new-output.txt	2025-09-17 07:23:59.584286258 +0000
@@ -316,16 +316,9 @@
 enums_behaviors.py:32:1: error[type-assertion-failure] Argument does not have asserted type `Literal[Color.BLUE]`
 enums_behaviors.py:44:21: error[subclass-of-final-class] Class `ExtendedShape` cannot inherit from final class `Shape`
 enums_expansion.py:52:9: error[type-assertion-failure] Argument does not have asserted type `CustomFlags`
-enums_member_names.py:21:1: error[type-assertion-failure] Argument does not have asserted type `Literal["RED"]`
-enums_member_names.py:22:1: error[type-assertion-failure] Argument does not have asserted type `Literal["RED"]`
-enums_member_names.py:26:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE"]`
 enums_member_names.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE", "GREEN"]`
-enums_member_values.py:21:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
-enums_member_values.py:22:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
-enums_member_values.py:26:5: error[type-assertion-failure] Argument does not have asserted type `Literal[1, 3]`
 enums_member_values.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal[1, 2, 3]`
 enums_member_values.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
-enums_member_values.py:68:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
 enums_member_values.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
 enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
 enums_members.py:129:9: error[type-assertion-failure] Argument does not have asserted type `Unknown`
@@ -869,5 +862,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 870 diagnostics
+Found 863 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-10 01:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
django-stubs (https://github.com/typeddjango/django-stubs)
- tests/assert_type/db/models/_enums.py:20:1: error[type-assertion-failure] Argument does not have asserted type `Literal["NORTH"]`
- tests/assert_type/db/models/test_enums.py:144:1: error[type-assertion-failure] Argument does not have asserted type `Literal["CLUB"]`
- tests/assert_type/db/models/test_enums.py:155:1: error[type-assertion-failure] Argument does not have asserted type `Literal["SENIOR"]`
- tests/assert_type/db/models/test_enums.py:167:1: error[type-assertion-failure] Argument does not have asserted type `Literal["CAR"]`
- tests/assert_type/db/models/test_enums.py:181:1: error[type-assertion-failure] Argument does not have asserted type `Literal["MALE"]`
- tests/assert_type/db/models/test_enums.py:194:1: error[type-assertion-failure] Argument does not have asserted type `Literal["GOLD"]`
- tests/assert_type/db/models/test_enums.py:207:1: error[type-assertion-failure] Argument does not have asserted type `Literal["FS"]`
- tests/assert_type/db/models/test_enums.py:221:1: error[type-assertion-failure] Argument does not have asserted type `Literal["PI"]`
- tests/assert_type/db/models/test_enums.py:235:1: error[type-assertion-failure] Argument does not have asserted type `Literal["ABYSS"]`
+ tests/assert_type/db/models/test_enums.py:237:1: error[type-assertion-failure] Argument does not have asserted type `Any`
- tests/assert_type/db/models/test_enums.py:248:1: error[type-assertion-failure] Argument does not have asserted type `Literal["NORTH"]`
- tests/assert_type/db/models/test_enums.py:260:1: error[type-assertion-failure] Argument does not have asserted type `Literal["NORTH"]`
- Found 476 diagnostics
+ Found 466 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/audit_logs.py:550:56: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (Unknown & ~None) | Literal[-1]`
+ discord/audit_logs.py:550:56: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (Unknown & ~None) | Literal[3, 4, 1, 5, -1]`
- discord/audit_logs.py:551:55: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (Unknown & ~None) | Literal[-1]`
+ discord/audit_logs.py:551:55: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (Unknown & ~None) | Literal[3, 4, 1, 5, -1]`

```
</details>
No memory usage changes detected ✅


---

_Comment by @thejchap on 2025-09-12 00:50_

@sharkdp could use some feedback on approach for `_value_`/`value` before i get further in

i am thinking to add a `value_ty` field to [EnumLiteralType](https://github.com/thejchap/ruff/blob/64c8a66390b14c6b3778a94dafa1a52eed82afc9/crates/ty_python_semantic/src/types.rs#L10209) that stores [this value](https://github.com/thejchap/ruff/blob/64c8a66390b14c6b3778a94dafa1a52eed82afc9/crates/ty_python_semantic/src/types/enums.rs#L123), which i think would require giving `EnumMetadata` the `'db` lifetime

anything im missing there or other recommendations?

---

_Comment by @sharkdp on 2025-09-12 07:26_

> i am thinking to add a `value_ty` field to [EnumLiteralType](https://github.com/thejchap/ruff/blob/64c8a66390b14c6b3778a94dafa1a52eed82afc9/crates/ty_python_semantic/src/types.rs#L10209) that stores [this value](https://github.com/thejchap/ruff/blob/64c8a66390b14c6b3778a94dafa1a52eed82afc9/crates/ty_python_semantic/src/types/enums.rs#L123)

I would rather expect that we add the value `Type` for enum members to `members` on `EnumMetadata`? Maybe by turning it to a `Name => Type` `HashMap`? Is that what you meant?

> which i think would require giving `EnumMetadata` the `'db` lifetime

Yes, it looks like that would require adding a lifetime parameter to `EnumMetadata`. 

> anything im missing there

I don't think so

---

_Comment by @codspeed-hq[bot] on 2025-09-13 03:14_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/thejchap%3Athejchap%2Fenum-members?runnerMode=Instrumentation)

### Merging #20311 will **not alter performance**

<sub>Comparing <code>thejchap:thejchap/enum-members</code> (78528f6) with <code>main</code> (d121a76)</sub>



### Summary

`✅ 43` untouched  





---

_@thejchap reviewed on 2025-09-13 12:35_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/enums.rs`:21 on 2025-09-13 12:35_

changed from `FxHashMap` to `BTreeMap` because `FxHashMap` doesn't implement `HashEqLike`, which it looks like is required for salsa to intern the struct

admittedly i am not 100% sure that interning the struct is necessary - i went down this road due to this error after adding the `'db` parameter to `EnumMetadata`, and it was the pattern i saw in other areas where structs with fields tied to the `'db` lifetime were getting returned from a tracked function (for example [GenericContext](https://github.com/thejchap/ruff/blob/1c247f9130837bf52fdc873479222be92523d6c3/crates/ty_python_semantic/src/types/generics.rs#L103))

```
62 | pub(crate) fn enum_metadata<'db>(
   |                             --- lifetime `'db` defined here
...
65 | ) -> Option<EnumMetadata<'db>> {
   |      ^^^^^^ requires that `'db` must outlive `'static`
```

for my own learning i'd appreciate a nudge in the right direction if this is the wrong approach, or a quick explanation as to why it was the right approach :)

---

_Comment by @github-actions[bot] on 2025-09-13 13:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[ty] infer type for enum members" to "[ty] infer `name` and `value` for enum members" by @thejchap on 2025-09-13 13:09_

---

_Marked ready for review by @thejchap on 2025-09-14 03:30_

---

_Review requested from @carljm by @thejchap on 2025-09-14 03:30_

---

_Review requested from @AlexWaygood by @thejchap on 2025-09-14 03:30_

---

_Review requested from @sharkdp by @thejchap on 2025-09-14 03:30_

---

_Review requested from @dcreager by @thejchap on 2025-09-14 03:30_

---

_@thejchap reviewed on 2025-09-14 03:47_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/enums.md`:186 on 2025-09-14 03:47_

https://diffswarm.dev/d-01k5331n13k9yx8tvnxnkeawp3?search=c-01k533qx21fstdzy6ys15rnjza

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-15 08:43_

---

_Comment by @github-actions[bot] on 2025-09-15 08:47_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 1 | 11 | 0 |
| `invalid-argument-type` | 0 | 0 | 2 |
| **Total** | **1** | **11** | **2** |


---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:186 on 2025-09-15 09:07_

Thank you for adding this test. Can we separate it from the one above (let it have it's own fenced code block) and add a small prose description above?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:37 on 2025-09-15 09:09_

Can we move these tests out of the "Basic" test, into a separate new section somewhere down below with a new section header?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:22 on 2025-09-15 09:10_

Similar to the comment below, let's maybe only keep `.value` and `.name` here and move the `_value_`/`_name_` tests to a dedicated test section below that deals with these attributes on enum members?

```suggestion
reveal_type(Color.RED.value)  # revealed: Literal[1]
reveal_type(Color.RED.name)  # revealed: Literal["RED"]
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3534 on 2025-09-15 09:11_

Is this necessary? (similar below)

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/builder.rs`:433 on 2025-09-15 09:13_

This might be the cause for the performance regression? Is there any way we can prevent cloning and `collect`ing here?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/enums.rs`:21 on 2025-09-15 09:15_

The alternative would be to derive `salsa::Update` for `EnumMetadata`. I think that should also get rid of this (very confusing and unhelpful) error message. That might be worth exploring to keep the diff in this PR smaller?

---

_@sharkdp reviewed on 2025-09-15 09:15_

Thank you very much. This looks great.

---

_@thejchap reviewed on 2025-09-16 18:26_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types.rs`:3534 on 2025-09-16 18:26_

without this, [these tests](https://github.com/astral-sh/ruff/pull/20311/files#diff-4ec0135eb3494415ddc8cee7f713314fcfcbc2b9d2722fa2292d75348791bd81R661) fail - i added this after reading through this [thread](https://github.com/astral-sh/ruff/pull/19481#issuecomment-3103460307)

---

_Review request for @carljm removed by @carljm on 2025-09-16 18:37_

---

_@thejchap reviewed on 2025-09-16 20:12_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/enums.rs`:21 on 2025-09-16 20:12_

required switching `FxOrderMap` to `FxIndexMap` because salsa [does not provide](https://github.com/salsa-rs/salsa/blob/master/src/update.rs#L322) an `Update` impl for `FxOrderMap`

---

_Review request for @dcreager removed by @dcreager on 2025-09-17 01:27_

---

_@sharkdp approved on 2025-09-17 07:22_

Thank you!

Glad we could get rid of the performance regression.

---

_Merged by @sharkdp on 2025-09-17 07:36_

---

_Closed by @sharkdp on 2025-09-17 07:36_

---

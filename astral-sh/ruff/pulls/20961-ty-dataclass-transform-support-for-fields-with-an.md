```yaml
number: 20961
title: "[ty] `dataclass_transform`: Support for fields with an `alias`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/dataclass_transform-alias
created_at: 2025-10-18T17:29:14Z
updated_at: 2025-10-18T18:34:44Z
url: https://github.com/astral-sh/ruff/pull/20961
synced_at: 2026-01-10T17:34:34Z
```

# [ty] `dataclass_transform`: Support for fields with an `alias`

---

_Pull request opened by @sharkdp on 2025-10-18 17:29_

## Summary

closes https://github.com/astral-sh/ty/issues/1385

## Conformance tests

Two false positives removed, as expected.

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-10-18 17:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-18 17:29_

---

_Renamed from "[ty] `dataclass_tranform`: Support for fields with an `alias`" to "[ty] `dataclass_transform`: Support for fields with an `alias`" by @sharkdp on 2025-10-18 17:29_

---

_Comment by @github-actions[bot] on 2025-10-18 17:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-18 18:18:00.160030501 +0000
+++ new-output.txt	2025-10-18 18:18:03.582057401 +0000
@@ -244,7 +244,6 @@
 dataclasses_postinit.py:29:7: error[unresolved-attribute] Type `DC1` has no attribute `y`
 dataclasses_slots.py:66:1: error[unresolved-attribute] Type `<class 'DC6'>` has no attribute `__slots__`
 dataclasses_slots.py:69:1: error[unresolved-attribute] Type `DC6` has no attribute `__slots__`
-dataclasses_transform_class.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
 dataclasses_transform_class.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_class.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_class.py:72:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
@@ -296,7 +295,6 @@
 dataclasses_transform_func.py:71:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
-dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
 dataclasses_transform_meta.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_meta.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
@@ -917,5 +915,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 919 diagnostics
+Found 917 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-18 17:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/commands/menu.py:608:19: error[missing-argument] No arguments provided for required parameters `_name`, `_type`
- tanjun/commands/menu.py:609:13: error[unknown-argument] Argument `type` does not match any known parameter
- tanjun/commands/menu.py:610:13: error[unknown-argument] Argument `name` does not match any known parameter
- tanjun/commands/menu.py:611:13: error[unknown-argument] Argument `name_localizations` does not match any known parameter
- tanjun/commands/menu.py:612:13: error[unknown-argument] Argument `is_nsfw` does not match any known parameter
- tanjun/context/autocomplete.py:236:13: error[missing-argument] No arguments provided for required parameters `_name`, `_value`
- tanjun/context/autocomplete.py:236:51: error[unknown-argument] Argument `name` does not match any known parameter
- tanjun/context/autocomplete.py:236:62: error[unknown-argument] Argument `value` does not match any known parameter
- tanjun/context/slash.py:647:17: error[unknown-argument] Argument `attachments` does not match any known parameter
- tanjun/context/slash.py:648:17: error[unknown-argument] Argument `components` does not match any known parameter
- tanjun/context/slash.py:649:17: error[unknown-argument] Argument `embeds` does not match any known parameter
- tanjun/context/slash.py:650:17: error[unknown-argument] Argument `flags` does not match any known parameter
- tanjun/context/slash.py:651:17: error[unknown-argument] Argument `is_tts` does not match any known parameter
- tanjun/context/slash.py:652:17: error[unknown-argument] Argument `mentions_everyone` does not match any known parameter
- tanjun/context/slash.py:653:17: error[unknown-argument] Argument `user_mentions` does not match any known parameter
- tanjun/context/slash.py:654:17: error[unknown-argument] Argument `role_mentions` does not match any known parameter
- Found 115 diagnostics
+ Found 99 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_asyncgens.py:232:36: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_core/_run.py:628:35: error[missing-argument] No argument provided for required parameter `_scope`
- src/trio/_core/_run.py:628:48: error[unknown-argument] Argument `scope` does not match any known parameter
- src/trio/_core/_run.py:628:60: error[unknown-argument] Argument `parent` does not match any known parameter
- src/trio/_core/_run.py:3046:26: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_cancelled.py:80:27: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_local.py:12:31: error[unknown-argument] Argument `default` does not match any known parameter
- src/trio/_core/_tests/test_local.py:41:31: error[unknown-argument] Argument `default` does not match any known parameter
- src/trio/_core/_tests/test_run.py:350:28: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:380:27: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:380:39: error[unknown-argument] Argument `relative_deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:383:27: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:385:27: error[unknown-argument] Argument `relative_deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:388:27: error[unknown-argument] Argument `relative_deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:485:44: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_core/_tests/test_run.py:603:36: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_core/_tests/test_run.py:622:28: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:762:31: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:1188:28: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:1821:28: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:1822:32: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_core/_tests/test_run.py:3004:32: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_file_io.py:295:31: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_subprocess.py:759:35: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_subprocess.py:760:50: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_sync.py:865:35: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_tests/test_threads.py:384:32: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_tests/test_threads.py:879:36: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_timeouts.py:28:29: error[unknown-argument] Argument `deadline` does not match any known parameter
- src/trio/_timeouts.py:28:48: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_timeouts.py:56:9: error[unknown-argument] Argument `shield` does not match any known parameter
- src/trio/_timeouts.py:57:9: error[unknown-argument] Argument `relative_deadline` does not match any known parameter
- Found 612 diagnostics
+ Found 580 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-18 17:35_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unknown-argument` | 0 | 45 | 0 |
| `missing-argument` | 0 | 3 | 0 |
| **Total** | **0** | **48** | **0** |

**[Full report with detailed diff](https://david-dataclass-transform-al.ecosystem-663.pages.dev/diff)** ([timing results](https://david-dataclass-transform-al.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @sharkdp on 2025-10-18 18:00_

---

_Review requested from @carljm by @sharkdp on 2025-10-18 18:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-18 18:00_

---

_Review requested from @dcreager by @sharkdp on 2025-10-18 18:00_

---

_Comment by @sharkdp on 2025-10-18 18:02_

I noticed that https://github.com/astral-sh/ty/issues/1159 is still not fixed after this PR (only one of the two errors goes away). But I don't think anything is wrong with `dataclass_transform` or `field_specifiers`. There's something wrong with overload resolution or generics, which causes us to infer unknown for calls to Pydantics `Field` if `default` and `alias` are both passed as keyword arguments. Everything works fine if `default` is passed positional-only or not at all. Will address/report this separately.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8005 on 2025-10-18 18:04_

It's possible using `Option<Name>` might lead to fewer allocations here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:662 on 2025-10-18 18:09_

```suggestion
                        let alias = alias
                            .and_then(Type::as_string_literal)
                            .map(|literal| Box::from(literal.value(db)));
```

(though this would need to be revised slightly if you do refactor to use `Option<Name>` instead of `Option<Box<str>>` 😄

---

_@AlexWaygood approved on 2025-10-18 18:09_

---

_@sharkdp reviewed on 2025-10-18 18:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:8005 on 2025-10-18 18:13_

Yes, but then `FieldInstance` itself will be larger for *every* field, not just those that use `alias`. I haven't measured it but I'd rather go with the more memory-conservative option here.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:568 on 2025-10-18 18:19_

It looks like we just ignore the `alias` argument if you try to pass a value for `alias` that's an instance of `str`, but isn't a string literal. We should probably emit a diagnostic for that, to warn the user that we can't figure out what they're trying to do? Though it's admittedly a bit of an edge case. E.g.

```py
from typing_extensions import dataclass_transform, Any

def field_with_alias(*, alias: str | None = None, kw_only: bool = False) -> Any: ...
@dataclass_transform(field_specifiers=(field_with_alias,))
def model[T](cls: type[T]) -> type[T]:
    return cls

def _(alias: str):
    @model
    class Person:
        internal_name: str = field_with_alias(alias=alias)
        internal_age: int = field_with_alias(alias=alias, kw_only=True)
    
    reveal_type(Person.__init__)
```

---

_@AlexWaygood reviewed on 2025-10-18 18:20_

---

_Merged by @sharkdp on 2025-10-18 18:20_

---

_Closed by @sharkdp on 2025-10-18 18:20_

---

_Branch deleted on 2025-10-18 18:20_

---

_@sharkdp reviewed on 2025-10-18 18:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:568 on 2025-10-18 18:34_

Right. Will write this down.

---

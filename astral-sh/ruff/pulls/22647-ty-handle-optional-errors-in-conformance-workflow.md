```yaml
number: 22647
title: "[ty] Handle optional errors in conformance workflow"
type: pull_request
state: merged
author: WillDuke
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: wld/handle-optional-errors-conformance
created_at: 2026-01-17T12:11:02Z
updated_at: 2026-01-17T12:47:45Z
url: https://github.com/astral-sh/ruff/pull/22647
synced_at: 2026-01-17T13:10:56Z
```

# [ty] Handle optional errors in conformance workflow

---

_@WillDuke_


<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Optional errors are now shown in separate `Added` and `Removed` detail sections and are excluded from statistics. I've also filtered the diagnostics from each `ty` version to exclude warnings, which prevents e.g. unused type ignore warnings from being reported as false positives.

## Test Plan

<!-- How was it tested? -->

I ran the updated script locally with the following command:

```
uv run --no-project scripts/conformance.py --tests-path ../typing/conformance/tests/ --old-ty uvx "ty@0.0.10"
```

Which produced the following output:

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 74.91% to 75.41%. The percentage of expected errors that received a diagnostic increased from 57.72% to 59.30%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 624 | 641 | +17 | ⏫ (✅) |
| False Positives | 209 | 209 | +0 |  |
| False Negatives | 457 | 440 | -17 | ⏬ (✅) |
| Total Diagnostics | 833 | 850 | +17 | ⏫ |
| Precision | 74.91% | 75.41% | +0.50% | ⏫ (✅) |
| Recall | 57.72% | 59.30% | +1.57% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [namedtuples_define_functional.py:16:8](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L16) | missing-argument | No argument provided for required parameter `y` |
| [namedtuples_define_functional.py:21:8](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L21) | missing-argument | No arguments provided for required parameters `x`, `y` |
| [namedtuples_define_functional.py:26:21](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L26) | too-many-positional-arguments | Too many positional arguments: expected 3, got 4 |
| [namedtuples_define_functional.py:31:8](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L31) | missing-argument | No argument provided for required parameter `y` |
| [namedtuples_define_functional.py:36:18](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L36) | invalid-argument-type | Argument is incorrect: Expected `int`, found `Literal["1"]` |
| [namedtuples_define_functional.py:37:21](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L37) | too-many-positional-arguments | Too many positional arguments: expected 3, got 4 |
| [namedtuples_define_functional.py:42:18](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L42) | invalid-argument-type | Argument is incorrect: Expected `int`, found `Literal["1"]` |
| [namedtuples_define_functional.py:43:15](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L43) | invalid-argument-type | Argument is incorrect: Expected `int`, found `float` |
| [namedtuples_define_functional.py:69:1](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L69) | missing-argument | No argument provided for required parameter `a` |
| [narrowing_typeguard.py:102:23](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L102) | invalid-type-guard-definition | `TypeGuard` function must have a parameter to narrow |
| [narrowing_typeguard.py:107:22](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L107) | invalid-type-guard-definition | `TypeGuard` function must have a parameter to narrow |
| [narrowing_typeis.py:105:23](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L105) | invalid-type-guard-definition | `TypeIs` function must have a parameter to narrow |
| [narrowing_typeis.py:110:22](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L110) | invalid-type-guard-definition | `TypeIs` function must have a parameter to narrow |
| [narrowing_typeis.py:195:27](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L195) | invalid-type-guard-definition | Narrowed type `str` is not assignable to the declared parameter type `int` |
| [narrowing_typeis.py:199:45](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L199) | invalid-type-guard-definition | Narrowed type `list[int]` is not assignable to the declared parameter type `list[object]` |
| [qualifiers_final_annotation.py:134:1](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py#L134) | missing-argument | No arguments provided for required parameters `x`, `y` |
| [qualifiers_final_annotation.py:135:3](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py#L135) | invalid-argument-type | Argument is incorrect: Expected `int`, found `Literal[""]` |


</details>

### Added (Optional)

<details>

| Location | Name | Message |
|----------|------|---------|
| [namedtuples_define_functional.py:52:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L52) | invalid-named-tuple | Duplicate field name `a` in `namedtuple()`: Field `a` already defined; will raise `ValueError` at runtime |
| [namedtuples_define_functional.py:53:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L53) | invalid-named-tuple | Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime |
| [namedtuples_define_functional.py:54:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L54) | invalid-named-tuple | Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime |
| [namedtuples_define_functional.py:55:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L55) | invalid-named-tuple | Field name `_d` in `namedtuple()` cannot start with an underscore: Will raise `ValueError` at runtime |


</details>

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 12:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Label `ci` added by @AlexWaygood on 2026-01-17 12:15_

---

_Label `ty` added by @AlexWaygood on 2026-01-17 12:15_

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:43 on 2026-01-17 12:17_

I see you like your functional programming :D

---

_@AlexWaygood reviewed on 2026-01-17 12:17_

---

_@WillDuke reviewed on 2026-01-17 12:18_

---

_Review comment by @WillDuke on `scripts/conformance.py`:43 on 2026-01-17 12:18_

Keeps it fun 😄 

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:118 on 2026-01-17 12:23_

```suggestion
                return "Optional Diagnostics Added"
            case Change.REMOVED:
                return "Optional Diagnostics Removed"
            case Change.UNCHANGED:
                return "Optional Diagnostics Unchanged"
```

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 12:23_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @AlexWaygood on `scripts/conformance.py`:156 on 2026-01-17 12:24_

It doesn't need to be in this PR, but it might be good if we could add some docs for the fields on this class at some point. `optional` is fairly clear, but I'd have to study the script a bit to figure out what e.g. `fingerprint` is 😄

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:452 on 2026-01-17 12:30_

I think in this case, you can do this in a slightly more readable way without the `tee` ;)

What about something like

```diff
diff --git a/scripts/conformance.py b/scripts/conformance.py
index f8e7124d8a..383c5eb6bf 100644
--- a/scripts/conformance.py
+++ b/scripts/conformance.py
@@ -435,19 +435,17 @@ def render_grouped_diagnostics(
             diag for diag in grouped if diag.change in (Change.ADDED, Change.REMOVED)
         ]
 
-    g1, g2 = tee(grouped)
-    required, optional = (
-        filterfalse(attrgetter("optional"), g1),
-        filter(attrgetter("optional"), g2),
-    )
-    required = sorted(
-        required,
-        key=attrgetter("classification"),
+    get_change = attrgetter("change")
+    get_classification = attrgetter("classification")
+
+    optional_diagnostics = sorted(
+        (diag for diag in grouped if diag.optional),
+        key=get_change,
         reverse=True,
     )
-    optional = sorted(
-        optional,
-        key=attrgetter("change"),
+    required_diagnostics = sorted(
+        (diag for diag in grouped if not diag.optional),
+        key=get_classification,
         reverse=True,
     )
 
@@ -466,8 +464,8 @@ def render_grouped_diagnostics(
 
     lines = []
     for group, diagnostics in chain(
-        groupby(required, key=attrgetter("classification")),
-        groupby(optional, key=attrgetter("change")),
+        groupby(required_diagnostics, key=get_classification),
+        groupby(optional_diagnostics, key=get_change),
     ):
         lines.append(f"### {group.into_title()}")
         lines.extend(["", "<details>", ""])
```

---

_@AlexWaygood reviewed on 2026-01-17 12:33_

Great stuff, thank you!

---

_@WillDuke reviewed on 2026-01-17 12:45_

---

_Review comment by @WillDuke on `scripts/conformance.py`:156 on 2026-01-17 12:45_

Makes sense. Regarding the fingerprint, I'm just preserving the data returned from `ty` in the gitlab output. I assume it provides a unique identifier for that diagnostic, but I don't actually know! Maybe it would be best to drop it since it is not used.

---

_@AlexWaygood approved on 2026-01-17 12:46_

Thank you!!

---

_Merged by @AlexWaygood on 2026-01-17 12:47_

---

_Closed by @AlexWaygood on 2026-01-17 12:47_

---

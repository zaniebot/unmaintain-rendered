```yaml
number: 22746
title: "[ty] Handle tagged errors in conformance"
type: pull_request
state: open
author: WillDuke
labels:
  - ci
  - ty
assignees: []
base: main
head: wld/handle-tagged-errors-conformance
created_at: 2026-01-19T21:38:32Z
updated_at: 2026-01-21T23:28:43Z
url: https://github.com/astral-sh/ruff/pull/22746
synced_at: 2026-01-22T00:08:57Z
```

# [ty] Handle tagged errors in conformance

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

I've added a second grouping step that handles tagged errors in the conformance suite. I decided to change the "unit" of a true positive or false positive to be a test case that is either a single error or a collection of tagged errors. Along with being more similar (I think) to the implementation in the typing repository, this approach also makes it easy to report all of the errors for a tag when e.g. there are two many for a tags requirements. The downside is that the statistics become a little more confusing.  

The essence of the new logic is to first collect all of the lines into mapping from (path, line) to tag name. I then add the same tag to each of the old and new diagnostics that appear on the same line. Next, I group the diagnostics based on a key that default to the path and line but will use the tag if it is present. An instance of `GroupedDiagnostics` will be considered a true positive, false positive etc. as a single unit, but it may be composed of multiple diagnostics from each of the ty versions and the conformance suite. The new `classify` method assigns the `GroupedDiagnostics` instance to one of these groups depending on the presence of each of the sources plus a new check to ensure that the correct number of distinct lines have errors within a tag.

This means that the classification doesn't penalize `ty` for raising multiple diagnostics on the same line even in cases where `ty` returns duplicate diagnostics. 

I've also updated the render step so that tagged diagnostics and diagnostics raised on the same line are now shown in the same cell in the table. The benefit here (I think) is that you'll be able to see all of the diagnostics removed when a line that raises multiple false positives is fixed.

## Test Plan

I ran the following locally:

```shell
uv run --no-project --with rich scripts/conformance.py --tests-path ../typing/conformance/ --old-ty uvx ty@0.0.6
```

<details>

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 74.16% to 76.75%. The percentage of expected errors that received a diagnostic increased from 61.57% to 64.25%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 620 | 647 | +27 | ⏫ (✅) |
| False Positives | 216 | 196 | -20 | ⏬ (✅) |
| False Negatives | 387 | 360 | -27 | ⏬ (✅) |
| Total Diagnostics | 910 | 920 | +10 | ⏫ |
| Precision | 74.16% | 76.75% | +2.59% | ⏫ (✅) |
| Recall | 61.57% | 64.25% | +2.68% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [namedtuples_define_functional.py:16:8](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L16) | missing-argument | No argument provided for required parameter `y` |
| [namedtuples_define_functional.py:21:8](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L21) | missing-argument | No arguments provided for required parameters `x`, `y` |
| [namedtuples_define_functional.py:26:21](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L26) | too-many-positional-arguments | Too many positional arguments: expected 3, got 4 |
| [namedtuples_define_functional.py:31:8](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L31)<br>[namedtuples_define_functional.py:31:18](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L31) | missing-argument<br>unknown-argument | No argument provided for required parameter `y`<br>Argument `z` does not match any known parameter |
| [namedtuples_define_functional.py:36:18](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L36) | invalid-argument-type | Argument is incorrect: Expected `int`, found `Literal["1"]` |
| [namedtuples_define_functional.py:37:21](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L37) | too-many-positional-arguments | Too many positional arguments: expected 3, got 4 |
| [namedtuples_define_functional.py:42:18](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L42) | invalid-argument-type | Argument is incorrect: Expected `int`, found `Literal["1"]` |
| [namedtuples_define_functional.py:43:15](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L43) | invalid-argument-type | Argument is incorrect: Expected `int`, found `float` |
| [namedtuples_define_functional.py:69:1](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L69) | missing-argument | No argument provided for required parameter `a` |
| [namedtuples_usage.py:43:5](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_usage.py#L43) | not-subscriptable | Cannot delete subscript on object of type `Point` with no `__delitem__` method |
| [narrowing_typeguard.py:102:23](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L102) | invalid-type-guard-definition | `TypeGuard` function must have a parameter to narrow |
| [narrowing_typeguard.py:107:22](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L107) | invalid-type-guard-definition | `TypeGuard` function must have a parameter to narrow |
| [narrowing_typeguard.py:128:20](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L128) | invalid-argument-type | Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeGuard[int]` |
| [narrowing_typeguard.py:148:26](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L148) | invalid-argument-type | Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeGuard[int]` |
| [narrowing_typeis.py:105:23](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L105) | invalid-type-guard-definition | `TypeIs` function must have a parameter to narrow |
| [narrowing_typeis.py:110:22](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L110) | invalid-type-guard-definition | `TypeIs` function must have a parameter to narrow |
| [narrowing_typeis.py:169:17](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L169) | invalid-argument-type | Argument to function `takes_typeguard` is incorrect: Expected `(object, /) -> TypeGuard[int]`, found `def is_int_typeis(val: object) -> TypeIs[int]` |
| [narrowing_typeis.py:170:14](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L170) | invalid-argument-type | Argument to function `takes_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def is_int_typeguard(val: object) -> TypeGuard[int]` |
| [narrowing_typeis.py:195:27](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L195) | invalid-type-guard-definition | Narrowed type `str` is not assignable to the declared parameter type `int` |
| [narrowing_typeis.py:199:45](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L199) | invalid-type-guard-definition | Narrowed type `list[int]` is not assignable to the declared parameter type `list[object]` |
| [qualifiers_final_annotation.py:134:1](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py#L134)<br>[qualifiers_final_annotation.py:134:3](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py#L134) | missing-argument<br>unknown-argument | No arguments provided for required parameters `x`, `y`<br>Argument `a` does not match any known parameter |
| [qualifiers_final_annotation.py:135:3](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py#L135)<br>[qualifiers_final_annotation.py:135:9](https://github.com/python/typing/blob/main/conformance/tests/qualifiers_final_annotation.py#L135) | invalid-argument-type<br>invalid-argument-type | Argument is incorrect: Expected `int`, found `Literal[""]`<br>Argument is incorrect: Expected `int`, found `Literal[""]` |
| [typeddicts_class_syntax.py:29:5](https://github.com/python/typing/blob/main/conformance/tests/typeddicts_class_syntax.py#L29) | invalid-typed-dict-statement | TypedDict class cannot have methods |
| [typeddicts_class_syntax.py:33:5](https://github.com/python/typing/blob/main/conformance/tests/typeddicts_class_syntax.py#L33) | invalid-typed-dict-statement | TypedDict class cannot have methods |
| [typeddicts_class_syntax.py:38:5](https://github.com/python/typing/blob/main/conformance/tests/typeddicts_class_syntax.py#L38) | invalid-typed-dict-statement | TypedDict class cannot have methods |
| [typeddicts_extra_items.py:128:15](https://github.com/python/typing/blob/main/conformance/tests/typeddicts_extra_items.py#L128) | invalid-argument-type | Cannot delete required key "name" from TypedDict `MovieEI` |
| [typeddicts_operations.py:49:11](https://github.com/python/typing/blob/main/conformance/tests/typeddicts_operations.py#L49) | invalid-argument-type | Cannot delete required key "name" from TypedDict `Movie` |


</details>

### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [constructors_call_init.py:25:1](https://github.com/python/typing/blob/main/conformance/tests/constructors_call_init.py#L25) | type-assertion-failure | Argument does not have asserted type `Class1[int \| float]` |
| [constructors_call_init.py:75:1](https://github.com/python/typing/blob/main/conformance/tests/constructors_call_init.py#L75) | type-assertion-failure | Argument does not have asserted type `Class5[int \| float]` |
| [constructors_call_new.py:24:1](https://github.com/python/typing/blob/main/conformance/tests/constructors_call_new.py#L24) | type-assertion-failure | Argument does not have asserted type `Class1[int \| float]` |
| [namedtuples_define_class.py:121:1](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_class.py#L121) | type-assertion-failure | Argument does not have asserted type `Property[int \| float]` |
| [namedtuples_define_class.py:122:1](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_class.py#L122) | type-assertion-failure | Argument does not have asserted type `int \| float` |
| [namedtuples_define_class.py:123:1](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_class.py#L123) | type-assertion-failure | Argument does not have asserted type `int \| float` |
| [narrowing_typeguard.py:17:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L17) | type-assertion-failure | Argument does not have asserted type `tuple[str, str]` |
| [narrowing_typeguard.py:32:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L32) | type-assertion-failure | Argument does not have asserted type `set[int]` |
| [narrowing_typeguard.py:69:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L69) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeguard.py:73:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L73) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeguard.py:77:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L77) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeguard.py:81:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L81) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeguard.py:85:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L85) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeguard.py:89:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L89) | type-assertion-failure | Argument does not have asserted type `B` |
| [narrowing_typeguard.py:93:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py#L93) | type-assertion-failure | Argument does not have asserted type `B` |
| [narrowing_typeis.py:72:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L72) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeis.py:76:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L76) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeis.py:80:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L80) | type-assertion-failure | Argument does not have asserted type `int` |
| [narrowing_typeis.py:92:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L92) | type-assertion-failure | Argument does not have asserted type `B` |
| [narrowing_typeis.py:96:9](https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeis.py#L96) | type-assertion-failure | Argument does not have asserted type `B` |


</details>

### Optional Diagnostics Added

<details>

| Location | Name | Message |
|----------|------|---------|
| [namedtuples_define_functional.py:52:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L52) | invalid-named-tuple | Duplicate field name `a` in `namedtuple()`: Field `a` already defined; will raise `ValueError` at runtime |
| [namedtuples_define_functional.py:53:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L53) | invalid-named-tuple | Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime |
| [namedtuples_define_functional.py:54:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L54) | invalid-named-tuple | Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime |
| [namedtuples_define_functional.py:55:25](https://github.com/python/typing/blob/main/conformance/tests/namedtuples_define_functional.py#L55) | invalid-named-tuple | Field name `_d` in `namedtuple()` cannot start with an underscore: Will raise `ValueError` at runtime |


</details>

</details>
<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-19 21:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 21:50_


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

_Label `ci` added by @AlexWaygood on 2026-01-19 22:08_

---

_Label `ty` added by @AlexWaygood on 2026-01-19 22:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-20 12:46_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2026-01-20 12:46_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2026-01-20 12:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-20 12:46_

---

_Converted to draft by @WillDuke on 2026-01-20 20:20_

---

_Marked ready for review by @WillDuke on 2026-01-21 09:21_

---

_Comment by @WillDuke on 2026-01-21 11:38_

@AlexWaygood @MichaReiser My first pass at this was a little buggy and overcomplicated, but I think that it is in a bit better shape now!

---

_Review comment by @MichaReiser on `scripts/conformance.py`:385 on 2026-01-21 14:40_

Ruff doesn't do a good job right now at formatting nested if-else expressions. But we can help it a bit to make this more readable by adding parentheses
```suggestion
                        tag=(
                        	f"{file.name}:{error.group('tag')}"
                        	if error.group("tag")
                        	else None
                      	),
```

---

_Review comment by @MichaReiser on `scripts/conformance.py`:471 on 2026-01-21 14:41_

Nit: I would use a regular loop here to avoid iterating `diagnostics` multiple times and also because I find it slightly more readable. But maybe that's the Rust dev in me.

```py
				old_diagnostics: list[Diagnostic] = []
        new_diagnostics: list[Diagnostic] = []
        expected_diagnostics: list[Diagnostic] = []
        sources: set[Source] = set()

        for diag in group:
            sources.add(diag.source)
            match diag.source:
                case Source.OLD:
                    old_diagnostics.append(diag)
                case Source.NEW:
                    new_diagnostics.append(diag)
                case Source.EXPECTED:
                    expected_diagnostics.append(diag)

        grouped = GroupedDiagnostics(
            key=key,
            sources=sources,
            old=old_diagnostics,
            new=new_diagnostics,
            expected=expected_diagnostics,
        )
```

---

_Review comment by @MichaReiser on `scripts/conformance.py`:482 on 2026-01-21 14:44_

It's a bit tricky to say what `num_errors` means in the presence of `E[tag+]` and `E?` where a single expected tag can have multiple errors and some errors are entirely optional. 

But it also seems that we never call `compute_stats` with `Source::Expected`. 

Should we remove this code and make `compute_stats` take a boolean argument instead (new_diagnostics)?

---

_Review comment by @MichaReiser on `scripts/conformance.py`:499 on 2026-01-21 14:46_

Nit: Maybe `non_optional_diagnostics`

---

_Review comment by @MichaReiser on `scripts/conformance.py`:430 on 2026-01-21 14:50_

GitHub doesn't really allow me to comment on that line but you could simplify your code a good amount by removing `None` from `old`, `new` and `expected`. Unless I miss a place where you create a `GroupedDiagnostics` instance where those fields are `None`

---

_Review comment by @MichaReiser on `scripts/conformance.py`:278 on 2026-01-21 14:53_

```suggestion
        elif source in self.sources:
```

I believe the second part is implicitly given by not taking the first if branch

It might also be more readable if you nested the conditions like so:

```py
if source in self.sources:
	if Source.EXPECTED in self.sources:
		distinct_lines = ... 

	else:
		return Classification.FalsePositive

elif Source.EXPECTED in self.sources:
	return Classification.FALSE_NEGATIVE

else: 
	return Classification.TRUE_NEGATIVE	
```

---

_Review comment by @MichaReiser on `scripts/conformance.py`:273 on 2026-01-21 14:54_

I can see how classifying too many diagnostics as false positives can be a bit confusing but I think it's the right thing. There's at least one false positive. 

It might be better to count them as both a true positive and false positive, but I suspect that this doesn't play well with how we use those numbers later on.

---

_Review comment by @MichaReiser on `scripts/conformance.py`:489 on 2026-01-21 14:59_

Nit: You could consider using a `match classification` here

---

_@MichaReiser approved on 2026-01-21 15:11_

Thank you. This overall makes sense to me. I've a few small nit comments

---

_@WillDuke reviewed on 2026-01-21 23:24_

---

_Review comment by @WillDuke on `scripts/conformance.py`:273 on 2026-01-21 23:24_

Makes sense. One advantage of grouping them like this is that you'll be able to see why an error that appears to match a line in the conformance suite is a false positive if another line with the same tag has a diagnostic.

---

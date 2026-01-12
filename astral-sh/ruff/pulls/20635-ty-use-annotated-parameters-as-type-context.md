```yaml
number: 20635
title: "[ty] Use annotated parameters as type context"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/argument-inference
created_at: 2025-09-29T18:06:04Z
updated_at: 2025-10-03T21:21:26Z
url: https://github.com/astral-sh/ruff/pull/20635
synced_at: 2026-01-12T15:57:06Z
```

# [ty] Use annotated parameters as type context

---

_@ibraheemdev_

## Summary

Use the type annotation of function parameters as bidirectional type context when inferring the argument expression. For example, the following example now type-checks:

```py
class TD(TypedDict):
    x: int

def f(_: TD): ...

f({ "x": 1 })
```

Part of https://github.com/astral-sh/ty/issues/168.


---

_Review requested from @carljm by @ibraheemdev on 2025-09-29 18:06_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-09-29 18:06_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-29 18:06_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-29 18:06_

---

_Label `ty` added by @ibraheemdev on 2025-09-29 18:06_

---

_Comment by @github-actions[bot] on 2025-09-29 18:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-03 20:48:40.319496259 +0000
+++ new-output.txt	2025-10-03 20:48:43.574517278 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(15c32)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16432)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-29 18:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paasta (https://github.com/yelp/paasta)
+ paasta_tools/utils.py:3416:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `(Unknown & ~None) | dict[str, Any]`
- Found 911 diagnostics
+ Found 912 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/tests/utils.py:351:17: error[invalid-assignment] Too many values to unpack: Expected 2
+ dulwich/tests/utils.py:353:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 195 diagnostics
+ Found 197 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
+ sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
- Found 319 diagnostics
+ Found 321 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ tests/GithubRetry.py:102:53: error[unsupported-operator] Operator `>` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[0]`
+ tests/GithubRetry.py:102:53: error[unsupported-operator] Operator `>` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[0]`
- Found 345 diagnostics
+ Found 347 diagnostics

flake8 (https://github.com/pycqa/flake8)
+ tests/unit/plugins/pycodestyle_test.py:30:48: error[unresolved-attribute] Type `ModuleType` has no attribute `lines`
+ tests/unit/plugins/pycodestyle_test.py:30:48: error[unresolved-attribute] Type `ModuleType` has no attribute `lines`
- Found 38 diagnostics
+ Found 40 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/config/_projects.py:534:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_parent` on type `T@from_hierarchy | Unknown`
- Found 313 diagnostics
+ Found 314 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/type_visitor.py:267:13: warning[redundant-cast] Value is already of type `Any`
+ mypy/type_visitor.py:282:13: warning[redundant-cast] Value is already of type `Any`
- Found 1832 diagnostics
+ Found 1834 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:1949:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `~int`
+ tanjun/annotations.py:1949:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- tanjun/annotations.py:1996:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `~int`
+ tanjun/annotations.py:1996:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
+ tanjun/annotations.py:2110:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `_NumberT@__getitem__ & ~float`
- Found 114 diagnostics
+ Found 115 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/backends/api.py:1691:26: warning[redundant-cast] Value is already of type `str`
- Found 1614 diagnostics
+ Found 1615 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/cmd/status.py:204:37: warning[possibly-missing-attribute] Attribute `items` on type `(Any & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (list[str] & ~AlwaysFalsy) | (dict[str, list[str]] & ~AlwaysFalsy)` may be missing
+ cloudinit/cmd/status.py:204:37: warning[possibly-missing-attribute] Attribute `items` on type `(Any & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (list[str] & ~AlwaysFalsy) | (dict[str, list[str]] & ~AlwaysFalsy)` may be missing
+ cloudinit/net/ephemeral.py:140:13: error[unresolved-attribute] Type `str` has no attribute `get`
- Found 836 diagnostics
+ Found 839 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/server/schemas/schedules.py:545:62: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_rrule`
+ src/prefect/server/schemas/schedules.py:545:62: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_rrule`
+ src/prefect/server/schemas/schedules.py:548:62: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_exrule`
+ src/prefect/server/schemas/schedules.py:548:62: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_exrule`
+ src/prefect/server/schemas/schedules.py:554:61: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_rdate`
+ src/prefect/server/schemas/schedules.py:554:61: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_rdate`
+ src/prefect/server/schemas/schedules.py:559:63: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_exdate`
+ src/prefect/server/schemas/schedules.py:559:63: error[unresolved-attribute] Type `rruleset & ~rrule` has no attribute `_exdate`
- Found 3189 diagnostics
+ Found 3197 diagnostics

vision (https://github.com/pytorch/vision)
+ references/detection/coco_utils.py:165:60: warning[possibly-unresolved-reference] Name `keypoints` used when possibly not defined
+ references/detection/coco_utils.py:165:60: warning[possibly-unresolved-reference] Name `keypoints` used when possibly not defined
- Found 1489 diagnostics
+ Found 1491 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'type' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'redirects' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'title_suggest' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'title_sort' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'subtitle' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'alternative_title' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'alternative_subtitle' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'edition_key' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'by_statement' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'publish_date' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'publish_year' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'first_edition' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'first_publisher' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'language' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'number_of_pages_median' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'lccn' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ia_box_id' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ia_loaded_id' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ia_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ia_collection' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'oclc' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'isbn' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ebook_access' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ebook_provider' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'lexile' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'lcc' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'lcc_sort' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ddc' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ddc_sort' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'contributor' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'publish_place' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'publisher' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'format' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'publisher_facet' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'first_sentence' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'author_alternative_name' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'author_facet' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'subject' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'subject_facet' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'subject_key' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'place' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'place_facet' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'place_key' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'person' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'person_facet' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'person_key' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'time' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'time_facet' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'time_key' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ratings_sortable' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ratings_count_1' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ratings_count_2' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ratings_count_3' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ratings_count_4' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ratings_count_5' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_0' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_1' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_2' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_3' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_4' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_5' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_6' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_7' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_8' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_9' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_10' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_11' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_12' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_13' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_14' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_15' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_16' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_17' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_18' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_19' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_20' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_21' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_22' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_23' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_hourly_sum' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_0' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_1' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_2' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_3' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_4' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_5' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_score_daily_6' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'trending_z_score' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'readinglog_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'currently_reading_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'already_read_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'osp_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'text' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'seed' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'name_str' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'alternate_names' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'birth_date' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'death_date' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'date' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'work_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'top_work' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'top_subjects' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'subject_type' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'last_modified' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'seed_count' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'printdisabled_s' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ia_collection_s' in TypedDict `SolrDocument` constructor
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[missing-typed-dict-key] Missing required key 'ebook_count_i' in TypedDict `SolrDocument` constructor
- Found 744 diagnostics
+ Found 853 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/core.py:356:10: warning[redundant-cast] Value is already of type `_PandasDataFrameT@sanitize_pandas_dataframe`
- Found 1337 diagnostics
+ Found 1336 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/core/hydpytools.py:2631:61: error[unresolved-attribute] Type `object` has no attribute `name`
- Found 608 diagnostics
+ Found 609 diagnostics

colour (https://github.com/colour-science/colour)
- colour/plotting/tm3018/report.py:347:40: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `str | None`
+ colour/plotting/tm3018/report.py:347:40: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:358:40: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `str | None`
+ colour/plotting/tm3018/report.py:358:40: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:372:47: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `str | None`
+ colour/plotting/tm3018/report.py:372:47: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:383:47: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `str | None`
+ colour/plotting/tm3018/report.py:383:47: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:425:30: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `str | None`
+ colour/plotting/tm3018/report.py:425:30: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/contrib/internal/pymongo/client.py:340:68: warning[possibly-unresolved-reference] Name `cursor` used when possibly not defined
+ ddtrace/contrib/internal/pymongo/client.py:340:68: warning[possibly-unresolved-reference] Name `cursor` used when possibly not defined
+ ddtrace/contrib/internal/unittest/patch.py:671:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` on type `CIVisibility | None` may be missing
+ ddtrace/contrib/internal/unittest/patch.py:671:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` on type `CIVisibility | None` may be missing
+ ddtrace/contrib/internal/unittest/patch.py:718:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` on type `CIVisibility | None` may be missing
+ ddtrace/contrib/internal/unittest/patch.py:718:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` on type `CIVisibility | None` may be missing
+ ddtrace/vendor/psutil/_compat.py:151:50: warning[possibly-unresolved-reference] Name `sorted_items` used when possibly not defined
+ ddtrace/vendor/psutil/_compat.py:151:50: warning[possibly-unresolved-reference] Name `sorted_items` used when possibly not defined
+ tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:36:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
+ tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:36:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
+ tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:71:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
+ tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:71:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'span_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'trace_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'parent_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'tags' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'start_ns' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'duration' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'status' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'meta' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key 'metrics' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[missing-typed-dict-key] Missing required key '_dd' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'parent_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'tags' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'start_ns' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'duration' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'status' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'meta' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key 'metrics' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[missing-typed-dict-key] Missing required key '_dd' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'parent_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'tags' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'start_ns' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'duration' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'status' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'meta' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key 'metrics' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[missing-typed-dict-key] Missing required key '_dd' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'span_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'trace_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'parent_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'tags' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'start_ns' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'duration' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'status' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'meta' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key 'metrics' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[missing-typed-dict-key] Missing required key '_dd' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'span_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'trace_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'parent_id' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'tags' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'start_ns' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'duration' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'status' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'meta' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key 'metrics' in TypedDict `LLMObsSpanEvent` constructor
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[missing-typed-dict-key] Missing required key '_dd' in TypedDict `LLMObsSpanEvent` constructor
- Found 7456 diagnostics
+ Found 7519 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/test_indexes.py:1305:23: error[unsupported-operator] Operator `+` is unsupported between objects of type `None` and `timedelta`
- tests/indexes/test_indexes.py:1307:23: error[unsupported-operator] Operator `-` is unsupported between objects of type `None` and `timedelta`
- tests/scalars/test_scalars.py:948:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:951:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:956:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:959:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:964:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:969:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:979:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:982:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:987:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:990:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:995:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1000:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1012:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1015:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1018:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1021:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1024:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1027:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1032:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1035:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1038:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1041:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1044:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1047:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1052:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1055:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1058:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1063:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1066:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1069:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1324:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1327:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1332:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1335:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1340:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1345:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1357:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1360:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1363:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1366:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1371:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1374:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1377:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1380:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1385:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1389:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1398:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1405:12: error[unresolved-attribute] Type `bool` has no attribute `all`
+ tests/scalars/test_scalars.py:1417:12: warning[possibly-missing-attribute] Attribute `all` on type `bool | @Todo` may be missing
- tests/scalars/test_scalars.py:1417:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1420:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1423:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1426:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1432:12: error[unresolved-attribute] Type `bool` has no attribute `all`
+ tests/scalars/test_scalars.py:1437:12: warning[possibly-missing-attribute] Attribute `all` on type `bool | @Todo` may be missing
- tests/scalars/test_scalars.py:1437:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1440:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1443:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1446:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1452:12: error[unresolved-attribute] Type `bool` has no attribute `all`
+ tests/scalars/test_scalars.py:1457:12: warning[possibly-missing-attribute] Attribute `all` on type `bool | @Todo` may be missing
- tests/scalars/test_scalars.py:1457:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1460:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1468:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1471:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1474:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1957:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1960:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1965:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1968:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1973:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1978:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1988:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1991:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1996:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1999:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2004:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2009:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2021:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2024:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2027:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2031:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2034:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2037:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2042:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2045:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2048:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2052:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2055:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2058:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2063:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2066:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2069:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2074:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2077:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:2080:12: error[unresolved-attribute] Type `bool` has no attribute `all`
- tests/series/test_series.py:533:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:534:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:535:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:536:17: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:539:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:540:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:541:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:542:17: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:545:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:546:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:547:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:548:17: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:551:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:552:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:553:23: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/series/test_series.py:554:17: error[unresolved-attribute] Type `None` has no attribute `sum`
- tests/test_frame.py:2640:14: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_frame.py:2644:14: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_frame.py:2649:14: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_frame.py:2654:14: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_frame.py:2661:14: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:301:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:306:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:311:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:316:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:326:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:331:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:336:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- tests/test_io.py:341:10: error[invalid-context-manager] Object of type `None` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ tests/test_io.py:987:13: error[type-assertion-failure] Argument does not have asserted type `dict[int | str, DataFrame]`
+ tests/test_pandas.py:476:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:478:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:479:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:481:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:482:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:484:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:485:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:489:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:490:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:493:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:494:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:497:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:498:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:501:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:502:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:505:18: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:506:22: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:513:15: error[type-assertion-failure] Argument does not have asserted type `str`
+ tests/test_pandas.py:515:15: error[type-assertion-failure] Argument does not have asserted type `str`
+ tests/test_pandas.py:517:9: error[type-assertion-failure] Argument does not have asserted type `NaTType | NAType | None`
+ tests/test_pandas.py:519:9: error[type-assertion-failure] Argument does not have asserted type `NaTType | NAType | None`
+ tests/test_pandas.py:523:15: error[type-assertion-failure] Argument does not have asserted type `int`
+ tests/test_pandas.py:525:15: error[type-assertion-failure] Argument does not have asserted type `int`
+ tests/test_pandas.py:527:15: error[type-assertion-failure] Argument does not have asserted type `None`
+ tests/test_pandas.py:529:15: error[type-assertion-failure] Argument does not have asserted type `None`
+ tests/test_pandas.py:533:15: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:535:15: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_pandas.py:537:9: error[type-assertion-failure] Argument does not have asserted type `NAType | None`
+ tests/test_pandas.py:539:9: error[type-assertion-failure] Argument does not have asserted type `NAType | None`
- Found 4910 diagnostics
+ Found 4819 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/pallas/fuser/block_spec.py:1517:39: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ jax/_src/pallas/fuser/block_spec.py:1517:39: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 2273 diagnostics
+ Found 2275 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/actions/message_edit.py:862:24: error[invalid-argument-type] Invalid argument to key "user_id" with declared type `int | None` on TypedDict `EditHistoryEvent`: value of type `Unknown | str | int`
+ zerver/actions/message_edit.py:863:26: error[invalid-argument-type] Invalid argument to key "timestamp" with declared type `int` on TypedDict `EditHistoryEvent`: value of type `Unknown | str | int`
+ zerver/views/auth.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | AnyStr@urljoin | None`
- Found 2718 diagnostics
+ Found 2721 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/crypto/crypto.py:2544:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | NegativeInfinity` and `Literal[1]`
+ sympy/crypto/crypto.py:2544:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | NegativeInfinity` and `Literal[1]`
+ sympy/functions/combinatorial/numbers.py:3193:61: warning[possibly-unresolved-reference] Name `s` used when possibly not defined
+ sympy/functions/combinatorial/numbers.py:3193:61: warning[possibly-unresolved-reference] Name `s` used when possibly not defined
+ sympy/ntheory/factor_.py:148:43: warning[possibly-unresolved-reference] Name `facs` used when possibly not defined
+ sympy/ntheory/factor_.py:148:43: warning[possibly-unresolved-reference] Name `facs` used when possibly not defined
+ sympy/polys/densetools.py:292:36: error[invalid-argument-type] Argument to function `dup_TC` is incorrect: Expected `Domain[Es@convert | Expr | int | float | complex | Er@dup_eval]`, found `Domain[Er@dup_eval]`
- Found 14007 diagnostics
+ Found 14014 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/history/events/structures/evm_event.py:204:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(str | None, /) -> Unknown`, found `def string_to_evm_address(value: str) -> @Todo`
+ rotkehlchen/history/events/structures/evm_event.py:204:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(str | None, /) -> Any`, found `def string_to_evm_address(value: str) -> @Todo`
- rotkehlchen/history/events/structures/evm_swap.py:103:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(str | None, /) -> Unknown`, found `def string_to_evm_address(value: str) -> @Todo`
+ rotkehlchen/history/events/structures/evm_swap.py:103:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(str | None, /) -> Any`, found `def string_to_evm_address(value: str) -> @Todo`

core (https://github.com/home-assistant/core)
+ homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
+ homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
+ homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
+ homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
+ homeassistant/components/caldav/todo.py:84:28: warning[possibly-unresolved-reference] Name `todo` used when possibly not defined
+ homeassistant/components/konnected/__init__.py:355:28: warning[possibly-unresolved-reference] Name `payload` used when possibly not defined
+ homeassistant/components/konnected/__init__.py:355:28: warning[possibly-unresolved-reference] Name `payload` used when possibly not defined
+ homeassistant/components/meteo_france/weather.py:194:21: error[missing-typed-dict-key] Missing required key 'datetime' in TypedDict `Forecast` constructor
+ homeassistant/components/meteo_france/weather.py:216:21: error[missing-typed-dict-key] Missing required key 'datetime' in TypedDict `Forecast` constructor
+ homeassistant/components/mqtt/debug_info.py:45:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/mqtt/debug_info.py:46:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/mqtt/debug_info.py:52:5: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/mqtt/debug_info.py:63:32: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/mqtt/debug_info.py:64:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/mqtt/debug_info.py:69:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/mqtt/debug_info.py:92:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | EntityDebugInfo | None` may be missing
+ homeassistant/components/random/config_flow.py:113:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
+ homeassistant/components/random/config_flow.py:113:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
+ homeassistant/components/sensor/__init__.py:607:59: warning[possibly-unresolved-reference] Name `classes` used when possibly not defined
+ homeassistant/components/sensor/__init__.py:607:59: warning[possibly-unresolved-reference] Name `classes` used when possibly not defined
+ homeassistant/components/template/config_flow.py:433:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
+ homeassistant/components/template/config_flow.py:433:82: warning[possibly-unresolved-reference] Name `units` used when possibly not defined
+ homeassistant/components/template/config_flow.py:457:54: warning[possibly-unresolved-reference] Name `state_classes` used when possibly not defined
+ homeassistant/components/template/config_flow.py:457:54: warning[possibly-unresolved-reference] Name `state_classes` used when possibly not defined
+ homeassistant/components/thread/diagnostics.py:151:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/thread/diagnostics.py:179:18: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/thread/diagnostics.py:200:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/todoist/calendar.py:570:21: error[unsupported-operator] Operator `>` is not supported for types `None` and `datetime`, in comparing `datetime | None` with `datetime`
+ homeassistant/components/todoist/calendar.py:552:21: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `bool`, in comparing `Unknown` with `Unknown | bool | str | None | list[Unknown] | datetime`
+ homeassistant/components/todoist/calendar.py:570:21: error[unsupported-operator] Operator `>` is not supported for types `bool` and `datetime`, in comparing `Unknown | bool | str | None | list[Unknown] | datetime` with `datetime`
- homeassistant/components/todoist/calendar.py:576:35: warning[possibly-missing-attribute] Attribute `date` on type `datetime | None` may be missing
+ homeassistant/components/todoist/calendar.py:576:35: warning[possibly-missing-attribute] Attribute `date` on type `Unknown | bool | str | None | list[Unknown] | datetime` may be missing
- homeassistant/components/todoist/calendar.py:579:20: error[unsupported-operator] Operator `<=` is not supported for types `None` and `datetime`, in comparing `datetime | None` with `datetime`
+ homeassistant/components/todoist/calendar.py:579:20: error[unsupported-operator] Operator `<=` is not supported for types `bool` and `str`, in comparing `Unknown | bool | str | None | list[Unknown] | datetime` with `Unknown | bool | str | None | list[Unknown] | datetime`
+ homeassistant/components/todoist/calendar.py:584:33: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | bool | str | None | list[Unknown] | datetime` and `timedelta`
- homeassistant/util/hass_dict.pyi:146:5: error[type-assertion-failure] Argument does not have asserted type `dict[str, int]`
- homeassistant/util/hass_dict.pyi:147:5: error[type-assertion-failure] Argument does not have asserted type `int`
- homeassistant/util/hass_dict.pyi:155:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/util/hass_dict.pyi:170:5: error[type-assertion-failure] Argument does not have asserted type `dict[str, int]`
- Found 13710 diagnostics
+ Found 13735 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/_lib/_util.py:1074:36: error[call-non-callable] Object of type `None` is not callable
+ scipy/_lib/_util.py:1074:36: error[call-non-callable] Object of type `None` is not callable
- Found 6758 diagnostics
+ Found 6760 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~33MB
+     struct fields = ~35MB

```
</details>


---

_Comment by @ibraheemdev on 2025-09-29 22:41_

It looks like a lot of these errors are duplicated diagnostics now that we infer argument expressions multiple times for callable unions/overloads, e.g.,

```py
if flag:
    def f(_: int): ...
else:
    def f(_: int): ...

if flag:
    x = 0

# warning[possibly-unresolved-reference] Name `x` used when possibly not defined
# warning[possibly-unresolved-reference] Name `x` used when possibly not defined
f(x)
```

(In this example, multi-inference is not strictly necessary because the signatures are identical, but that's a separate performance concern).

I'm not sure there's a great way around this other than adding checks around all diagnostics that aren't type-context specific?

---

_@ibraheemdev reviewed on 2025-09-29 22:57_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:9465 on 2025-09-29 22:57_

The reason we even eagerly report diagnostics during the inference of `TypedDict` literals in the first place, is because a later assignability diagnostic would not have context for missing or invalid key diagnostics. Otherwise, I don't think we should be emitting diagnostics based on the type context, which acts more as a type hint (and is not necessarily required to be assignable to, e.g. when there is a fallback overload).

If we had an internal implicit `TypedDict` representation we may be able to avoid this, though we also wouldn't even need bidirectional type inference for `TypedDict` literals in that case.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-30 06:55_

---

_Comment by @github-actions[bot] on 2025-09-30 07:00_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `missing-typed-dict-key` | 162 | 0 | 0 |
| `unresolved-attribute` | 2 | 109 | 0 |
| `type-assertion-failure` | 30 | 3 | 0 |
| `invalid-argument-type` | 4 | 0 | 9 |
| `invalid-context-manager` | 0 | 13 | 0 |
| `non-subscriptable` | 9 | 0 | 0 |
| `unsupported-operator` | 2 | 2 | 2 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `redundant-cast` | 3 | 1 | 0 |
| `invalid-assignment` | 3 | 0 | 0 |
| `invalid-return-type` | 2 | 0 | 0 |
| `possibly-missing-implicit-call` | 1 | 0 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **221** | **129** | **12** |

**[Full report with detailed diff](https://ibraheem-argument-inference.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-argument-inference.ecosystem-663.pages.dev/timing))


---

_Comment by @sharkdp on 2025-09-30 11:27_

> It looks like a lot of these errors are duplicated diagnostics now that we infer argument expressions multiple times for callable unions/overloads […]  I'm not sure there's a great way around this other than adding checks around all diagnostics that aren't type-context specific?

Instead of adding checks for every non-type-context-specific context, can we use state on `InferContext` to keep track of whether diagnostics should be emitted or not (see the `InferContext::no_type_check` field for a similar functionality)? And then, we could circumvent the general silencing of all diagnostics for those diagnostics that should still be emitted?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:9465 on 2025-09-30 11:44_

> The reason we even eagerly report diagnostics during the inference of `TypedDict` literals in the first place, is because a later assignability diagnostic would not have context for missing or invalid key diagnostics.

Correct

> Otherwise, I don't think we should be emitting diagnostics based on the type context, which acts more as a type hint (and is not necessarily required to be assignable to, e.g. when there is a fallback overload).

:+1: 

> If we had an internal implicit `TypedDict` representation we may be able to avoid this, though we also wouldn't even need bidirectional type inference for `TypedDict` literals in that case.

Yes, although Carl pointed out that this approach would be tricky as well, because we would need to keep track of mutations (including through aliases):

```py
class Person(TypedDict):
    name: str
    age: int

def takes_person(p: Person): ...

# creates ImplicitTypedDict {"name": Literal["Alice"], "age": Literal[20] }
alice = {"name": "Alice", "age": 20}

takes_person(alice)  # okay

evil = alice
evil["age"] = None

takes_person(alice)  # not okay anymore
```

But on the other hand, this is a case that the current approach can't handle either, because `alice` would just be a `dict[Unknown | str, Unknown | str | int]`.

---

_@sharkdp reviewed on 2025-09-30 11:44_

---

_Comment by @codspeed-hq[bot] on 2025-09-30 21:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fargument-inference?runnerMode=Instrumentation)

### Merging #20635 will **degrade performances by 17.12%**

<sub>Comparing <code>ibraheem/argument-inference</code> (dcd1dc5) with <code>main</code> (b83ac5e)</sub>



### Summary

`❌ 1` regression  
`✅ 42` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fargument-inference?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` hydra-zen `` | 764.5 ms | 922.4 ms | -17.12% |


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-30 23:38_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-30 23:38_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-30 23:50_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-30 23:50_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/context.rs`:188 on 2025-10-01 06:24_

I think we can wait with adding this method. `report_diagnostic` is only used for errors that can't be suppressed and shouldn't be specific to bidirectional type inference.

---

_@MichaReiser reviewed on 2025-10-01 06:24_

---

_Comment by @codspeed-hq[bot] on 2025-10-02 19:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fargument-inference?runnerMode=WallTime)

### Merging #20635 will **degrade performances by 21.13%**

<sub>Comparing <code>ibraheem/argument-inference</code> (dcd1dc5) with <code>main</code> (b83ac5e)</sub>



### Summary

`❌ 5` regressions  
`✅ 3` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fargument-inference?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 40.6 s | 42.8 s | -5.15% |
| ❌ | `` medium[colour-science] `` | 9.1 s | 11.5 s | -21.13% |
| ❌ | `` medium[pandas] `` | 32.2 s | 34.3 s | -6.07% |
| ❌ | `` medium[static-frame] `` | 8.9 s | 9.4 s | -4.82% |
| ❌ | `` small[pydantic] `` | 2.5 s | 2.6 s | -4.26% |


---

_Comment by @github-actions[bot] on 2025-10-02 19:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-03 01:06_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-03 01:06_

---

_Comment by @ibraheemdev on 2025-10-03 04:01_

I've decided to remove the idea of "lints that may be affected by bidirectional type context", as this seemed quite. Instead, the new implementation does the following for each argument:
- Performs type inference directly with the unique annotated parameter type, if there is a single binding and overload.
- Otherwise, performs type inference once without type context. This will surface any diagnostics from inference of the expression. We then perform type inference once for each overload, with its annotated parameter type, with diagnostics silenced. The intersection of all these types are taken — so the type context can potentially allow us to infer a more assignable type, but will never lead to any new diagnostics, as we don't know which overload will match during argument inference.

This does mean we get worse diagnostics for `TypedDict` arguments when there are multiple bindings/overloads involved, due to the fallback to a generic `dict` (once `TypedDict` subtyping is properly implemented, of course), but I don't think there's really a way around this, and it doesn't seem like a priority.

I also went through the ecosystem report, and most diagnostic changes seem to be correct, or the result of an unrelated known limitation (namely `TypedDict` subtyping, and the new constraint solver). This PR should be ready for review.

---

_@ibraheemdev reviewed on 2025-10-03 04:18_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/call/bind.rs`:2517 on 2025-10-03 04:18_

This is mostly for better diagnostics. Without it, `reveal_type(identity(1))` is revealed as `T@reveal_type | int`. It also doesn't quite work for parameters annotated as `T | None` — we still might union the inferred type with `T@f` in that case.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:3671 on 2025-10-03 18:02_

nit: I think you can update this helper to take in `argument_index: usize`, and use `argument_index?` up above when you call the helper. Then you only need to match on `node` below.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:4989 on 2025-10-03 18:06_

nit: Can you use `std::iter::once` here? I find the array notation makes me assume there will be multiple elements, which is subtle since there's a tuple here, too.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4199 on 2025-10-03 18:19_

Were there tests that were failing with the old signature? I can see that the new signature means that there's now a return value, which will have the same type as the value being checked, but I don't see any tests that rely on that behavior.

---

_@dcreager approved on 2025-10-03 18:21_

---

_@ibraheemdev reviewed on 2025-10-03 20:25_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:4199 on 2025-10-03 20:25_

The issue that showed up in the ecosystem report was that the `f(1)` in `assert_type(f(1), int)` was inferred as `int | Any`, because the signature of `assert_type` was `(Any, ..) -> ..`. I added a test.

---

_@ibraheemdev reviewed on 2025-10-03 20:32_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/call/bind.rs`:3671 on 2025-10-03 20:32_

We also use `get_argument_node` in the diagnostic code with an `Option<usize>`, so I'm not sure the change is worth it.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-03 21:01_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-03 21:01_

---

_Merged by @ibraheemdev on 2025-10-03 21:14_

---

_Closed by @ibraheemdev on 2025-10-03 21:14_

---

_Branch deleted on 2025-10-03 21:14_

---

_Comment by @ibraheemdev on 2025-10-03 21:21_

Deduplicating the parameter types helps the performance regression somewhat, but outside of that I think it's unavoidable.

---

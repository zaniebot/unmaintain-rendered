```yaml
number: 21205
title: "[ty] Update to the latest version of the conformance suite"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/bump-conformance-suite-sha
created_at: 2025-11-02T13:38:33Z
updated_at: 2025-11-02T16:33:33Z
url: https://github.com/astral-sh/ruff/pull/21205
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Update to the latest version of the conformance suite

---

_Pull request opened by @sharkdp on 2025-11-02 13:38_

## Summary

There have been some larger-scale updates to the conformance suite since we introduced our CI job, so it seems sensible to bump the version of the conformance suite to the latest state.

## Test plan

This is a bit awkward to test. Here is the diff of running ty on the conformance suite before and after this bump. I filtered out line/column information (`sed -re 's/\.py:[0-9]+:[0-9]+:/.py/'`) to avoid spurious changes from content that has simply been moved around.

```diff
1,2c1
< fatal[panic] Panicked at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/shark/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1a99c)): execute: too many cycle iterations`
< src/type_checker.py error[unresolved-import] Cannot resolve imported module `tqdm`
---
> fatal[panic] Panicked at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/shark/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(6e4c)): execute: too many cycle iterations`
205,206d203
< tests/constructors_call_metaclass.py error[type-assertion-failure] Argument does not have asserted type `Never`
< tests/constructors_call_metaclass.py error[missing-argument] No argument provided for required parameter `x` of function `__new__`
268a266,273
> tests/dataclasses_match_args.py error[type-assertion-failure] Argument does not have asserted type `tuple[Literal["x"]]`
> tests/dataclasses_match_args.py error[unresolved-attribute] Class `DC1` has no attribute `__match_args__`
> tests/dataclasses_match_args.py error[type-assertion-failure] Argument does not have asserted type `tuple[Literal["x"]]`
> tests/dataclasses_match_args.py error[unresolved-attribute] Class `DC2` has no attribute `__match_args__`
> tests/dataclasses_match_args.py error[type-assertion-failure] Argument does not have asserted type `tuple[Literal["x"]]`
> tests/dataclasses_match_args.py error[unresolved-attribute] Class `DC3` has no attribute `__match_args__`
> tests/dataclasses_match_args.py error[unresolved-attribute] Class `DC4` has no attribute `__match_args__`
> tests/dataclasses_match_args.py error[type-assertion-failure] Argument does not have asserted type `tuple[()]`
339a345
> tests/directives_assert_type.py error[type-assertion-failure] Argument does not have asserted type `Any`
424a431
> tests/generics_defaults.py error[type-assertion-failure] Argument does not have asserted type `Any`
520a528,529
> tests/generics_syntax_infer_variance.py error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@ShouldBeCovariant2 | Sequence[T@ShouldBeCovariant2]`
> tests/generics_syntax_infer_variance.py error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
711a721
> tests/namedtuples_define_class.py error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
795d804
< tests/protocols_explicit.py error[invalid-attribute-access] Cannot assign to ClassVar `cm1` from an instance of type `Self@__init__`
822,823d830
< tests/qualifiers_annotated.py error[invalid-syntax] named expression cannot be used within a type annotation
< tests/qualifiers_annotated.py error[invalid-syntax] await expression cannot be used within a type annotation
922a930,953
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "novel_adaptation"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "year"
> tests/typeddicts_extra_items.py error[type-assertion-failure] Argument does not have asserted type `bool`
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "novel_adaptation"
> tests/typeddicts_extra_items.py error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `InheritedMovie`: value of type `None`
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `InheritedMovie`: Unknown key "other_extra_key"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `MovieEI`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `MovieExtraInt`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `MovieExtraStr`: Unknown key "description"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `MovieExtraInt`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `NonClosedMovie`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `ExtraMovie`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `ExtraMovie`: Unknown key "language"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `ClosedMovie`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `MovieExtraStr`: Unknown key "summary"
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `MovieExtraInt`: Unknown key "year"
> tests/typeddicts_extra_items.py error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | int]` is not assignable to `Mapping[str, int]`
> tests/typeddicts_extra_items.py error[type-assertion-failure] Argument does not have asserted type `list[tuple[str, int | str]]`
> tests/typeddicts_extra_items.py error[type-assertion-failure] Argument does not have asserted type `list[int | str]`
> tests/typeddicts_extra_items.py error[unresolved-attribute] Object of type `IntDict` has no attribute `clear`
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `IntDictWithNum`: Unknown key "bar" - did you mean "num"?
> tests/typeddicts_extra_items.py error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
> tests/typeddicts_extra_items.py error[invalid-key] Cannot access `IntDictWithNum` with a key of type `str`. Only string literals are allowed as keys on TypedDicts.
> tests/typeddicts_extra_items.py error[invalid-key] Invalid key for TypedDict `IntDictWithNum` of type `str`
950c981
< Found 949 diagnostics
---
> Found 980 diagnostics
```

---

_Label `internal` added by @sharkdp on 2025-11-02 13:38_

---

_Label `ty` added by @sharkdp on 2025-11-02 13:38_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-02 13:38_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-02 13:38_

---

_Comment by @github-actions[bot] on 2025-11-02 13:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-02 13:43_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-bump-conformance-suite.ecosystem-663.pages.dev/diff)** ([timing results](https://david-bump-conformance-suite.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @sharkdp on 2025-11-02 13:49_

---

_@MichaReiser approved on 2025-11-02 14:41_

---

_Merged by @sharkdp on 2025-11-02 16:33_

---

_Closed by @sharkdp on 2025-11-02 16:33_

---

_Branch deleted on 2025-11-02 16:33_

---

```yaml
number: 22649
title: "conformance.py: Only use the \"No changes detected ✅\" summary if no diagnostics were added or removed"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
base: main
head: alex/conformance-summary
created_at: 2026-01-17T13:03:38Z
updated_at: 2026-01-17T13:04:08Z
url: https://github.com/astral-sh/ruff/pull/22649
synced_at: 2026-01-17T13:10:56Z
```

# conformance.py: Only use the "No changes detected ✅" summary if no diagnostics were added or removed

---

_@AlexWaygood_

## Summary

Currently if there are no top-line changes to the statistics, we display a "No changes detected ✅" top-line summary. But that's a bit weird in situations like https://github.com/astral-sh/ruff/pull/22644#issuecomment-3762939568, where there clearly _were_ changes detected, they just didn't impact the top-line statistics at all (because exactly the same number of false positives were added as there were false positives removed):

![Uploading image.png…]()

This PR changes the logic in `conformance.py` so that we only use the "No changes detected ✅" summary if there really were no changes detected!

## Test Plan

I:
- Checked out the `main` branch and ran `cargo build -p ty && mv ./target/debug/ty ./target/debug/ty-old`
- Checked out the branch for #22644 and ran `cargo build -p ty && mv ./target/debug/ty ./target/debug/ty-new`
- Checked out this branch again and ran `uv run --no-project scripts/conformance.py --old-ty=./target/debug/ty-old --new-ty=./target/debug/ty-new --tests-path=../typing/conformance/tests --output=results.md`
- Observed that the following MarkDown was generated:

---

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/)

The percentage of diagnostics emitted that were expected errors held steady at 75.50%. The percentage of expected errors that received a diagnostic held steady at 59.30%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 641 | 641 | +0 |  |
| False Positives | 208 | 208 | +0 |  |
| False Negatives | 440 | 440 | +0 |  |
| Total Diagnostics | 849 | 849 | +0 |  |
| Precision | 75.50% | 75.50% | +0.00% |  |
| Recall | 59.30% | 59.30% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [aliases_type_statement.py:10:52](https://github.com/python/typing/blob/main/conformance/tests/aliases_type_statement.py#L10) | invalid-type-arguments | Too many type arguments: expected 2, got 3 |
| [aliases_typealiastype.py:23:5](https://github.com/python/typing/blob/main/conformance/tests/aliases_typealiastype.py#L23) | invalid-argument-type | Argument to class `TypeAliasType` is incorrect: Expected `tuple[TypeVar \| ParamSpec \| typing_extensions.TypeVarTuple, ...]`, found `tuple[TypeVar, TypeVar, ParamSpec, typing.TypeVarTuple]` |


</details>

### False positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_defaults.py:88:39](https://github.com/python/typing/blob/main/conformance/tests/generics_defaults.py#L88) | invalid-legacy-type-variable | The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13 |
| [generics_defaults.py:99:53](https://github.com/python/typing/blob/main/conformance/tests/generics_defaults.py#L99) | invalid-legacy-type-variable | The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13 |


</details>


---

_Label `ci` added by @AlexWaygood on 2026-01-17 13:03_

---

_Label `ty` added by @AlexWaygood on 2026-01-17 13:03_

---

_Comment by @AlexWaygood on 2026-01-17 13:04_

@WillDuke, I don't think I can request your review here, but I'd love your feedback if you have time!

---

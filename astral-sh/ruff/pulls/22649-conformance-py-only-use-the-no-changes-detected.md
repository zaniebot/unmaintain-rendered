```yaml
number: 22649
title: "conformance.py: Only use the \"No changes detected ✅\" summary if no diagnostics were added or removed"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/conformance-summary
created_at: 2026-01-17T13:03:38Z
updated_at: 2026-01-17T13:30:46Z
url: https://github.com/astral-sh/ruff/pull/22649
synced_at: 2026-01-17T14:11:29Z
```

# conformance.py: Only use the "No changes detected ✅" summary if no diagnostics were added or removed

---

_@AlexWaygood_

## Summary

Currently if there are no top-line changes to the statistics, we display a "No changes detected ✅" top-line summary. But that's a bit weird in situations like https://github.com/astral-sh/ruff/pull/22644#issuecomment-3762939568, where there clearly _were_ changes detected, they just didn't impact the top-line statistics at all (because exactly the same number of false positives were added as there were false positives removed):

<img width="1894" height="772" alt="image" src="https://github.com/user-attachments/assets/ec147aca-e0e5-43f5-9ce5-2e71a6780312" />

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

_Comment by @astral-sh-bot[bot] on 2026-01-17 13:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-17 13:12_


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

_Comment by @WillDuke on 2026-01-17 13:26_

> @WillDuke, I don't think I can request your review here, but I'd love your feedback if you have time!

This looks good to me!

---

_Merged by @AlexWaygood on 2026-01-17 13:30_

---

_Closed by @AlexWaygood on 2026-01-17 13:30_

---

_Branch deleted on 2026-01-17 13:30_

---

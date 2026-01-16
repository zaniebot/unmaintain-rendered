```yaml
number: 22427
title: "[ruff]: add flake8-annotation-complexity"
type: pull_request
state: open
author: danjones1618
labels:
  - rule
assignees: []
base: main
head: feat-rule-flake8-annoation-complexity
created_at: 2026-01-06T22:57:30Z
updated_at: 2026-01-16T02:25:09Z
url: https://github.com/astral-sh/ruff/pull/22427
synced_at: 2026-01-16T03:04:49Z
```

# [ruff]: add flake8-annotation-complexity

---

_@danjones1618_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds support for [flake8-annotation-complexity](https://github.com/best-doctor/flake8-annotations-complexity) as requested by https://github.com/astral-sh/ruff/issues/2323.

As per the disucssion in #2323 the following has been implemented:

- Adds TAE002 to mirror `flake8-annotations-complexity`
- Ignores `typing.Annotated` for the purpose of annotation complexity
- Considers PEP-604 union syntax as complex as `Union` (+1)

Before merging, we should round-off any discussion over the formatting of the rules and associated codes. Currently it's adhering to the upstream however we may wish to split this into multiple rules like: `complex-argument-type-annotation`, `complex-return-type-annotation`, `complex-variable-annotation`, and so forth. In this case, we should move these to be under the `RUF` group instead of adding the `TAE` group.

## Test Plan

<!-- How was it tested? -->

- Added unit tests
- Added integration snapshot tests


---

_Renamed from "feat: implement initial complexity calculation" to "[ruff]: add flake8-annotation-complexity" by @danjones1618 on 2026-01-06 22:58_

---

_Marked ready for review by @danjones1618 on 2026-01-15 23:35_

---

_Label `rule` added by @amyreese on 2026-01-16 02:04_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:36 on 2026-01-16 02:10_

Will need to update this once ready to land.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:10 on 2026-01-16 02:10_

```suggestion
/// Checks for type annotation which are too complex.
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:13 on 2026-01-16 02:11_

```suggestion
/// Annotation complexity is a symptom of using generic types for complex, nested data structures.
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:78 on 2026-01-16 02:13_

```suggestion
struct CheckerAnnotationResolver<'a, 'b>
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:58 on 2026-01-16 02:13_

```suggestion
    fn resolve_annotation_qualified_name<'a, 'expr>(
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:67 on 2026-01-16 02:15_

```suggestion
    fn is_annotated_type(&self, expr: &Expr) -> bool {
```

We might already have a helper for this related to the annotated type handling in fastapi/etc.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:121 on 2026-01-16 02:17_

```suggestion
    if let Some(expr) = expr.as_string_literal_expr()
    && let Some(literal_value) = expr.as_single_part_string()
    && let Ok(inner_expr) = parse_expression(&literal_value.value) {
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:121 on 2026-01-16 02:17_

Prefer let chains rather than nested forms.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:171 on 2026-01-16 02:19_

```suggestion
            let annotation_complexity =
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:211 on 2026-01-16 02:20_

```suggestion
    let annotation_resolver = CheckerAnnoationResolver { checker };

    let annotation_complexity =
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:239 on 2026-01-16 02:21_

```suggestion
        fn resolve_annotation_qualified_name<'a, 'expr>(
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:271 on 2026-01-16 02:21_

```suggestion
    fn test_get_annotation_complexity_yields_expected_value(
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_annotation_complexity/snapshots/ruff_linter__rules__flake8_annotation_complexity__tests__TAE002_yields_errors_TAE002.py.snap`:29 on 2026-01-16 02:22_

Limit the range to just the annotation to match the diagnostics for parameters and return types?

---

_@amyreese requested changes on 2026-01-16 02:24_

---

_Review requested from @ntBre by @amyreese on 2026-01-16 02:25_

---

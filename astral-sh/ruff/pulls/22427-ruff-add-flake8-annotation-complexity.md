```yaml
number: 22427
title: "[ruff]: add flake8-annotation-complexity"
type: pull_request
state: open
author: danjones1618
labels:
  - rule
  - preview
assignees: []
base: main
head: feat-rule-flake8-annoation-complexity
created_at: 2026-01-06T22:57:30Z
updated_at: 2026-01-17T00:01:26Z
url: https://github.com/astral-sh/ruff/pull/22427
synced_at: 2026-01-17T00:08:11Z
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

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_annotation_complexity/TAE002_quoted.py`:1 on 2026-01-16 21:42_

This is a small nit, but I would probably just include ~1 test case with a quoted annotation and keep it in the same file as the other tests. I think that should be enough to demonstrate that the rule applies to quoted annotations, otherwise I wouldn't expect there to be any differences in behavior.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:36 on 2026-01-16 21:44_

0.14.14 would be the current version to include here, assuming it lands before next Thursday.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:28 on 2026-01-16 21:45_

I think it might be worth suggesting a type alias too, that's what clippy does for its similar lint: https://rust-lang.github.io/rust-clippy/master/index.html?search=type_#type_complexity. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/snapshots/ruff_linter__rules__flake8_annotation_complexity__tests__TAE002_yields_errors_TAE002.py.snap`:29 on 2026-01-16 21:48_

Agreed, I think this would look better as:


```suggestion
21 | variable_with_complex_ann: dict[str, dict[str, dict[str, dict[str, str]]]] = {}
   |                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:1023 on 2026-01-16 21:53_

I think this should just be `int` in this attribute, at least that's what we have for another complexity metric:

https://github.com/astral-sh/ruff/blob/5c97b6ef40cd7fb34c2c69619657ef150d6031f4/crates/ruff_workspace/src/options.rs#L2907-L2915


```suggestion
        value_type = "int",
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:40 on 2026-01-16 21:54_

Can these ever be negative? If not, I would usually default to a `usize`, like the other complexity metric I linked to below.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:161 on 2026-01-16 21:55_

Let's move this function and the assignment version closer to the top of the file, just below the `Violation` struct and implementation.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:54 on 2026-01-16 21:57_

I think it would make sense to implement a `fix_title` function too (even if we can't provide an actual fix). That will attach a `help` sub-diagnostic to the end of the main diagnostic, where we could put a message like `consider using a type alias` or something. Just an idea.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:99 on 2026-01-16 21:59_

I haven't really looked closely enough to see if this is a drop-in replacement, but this function reminds me of an existing helper:

https://github.com/astral-sh/ruff/blob/5c97b6ef40cd7fb34c2c69619657ef150d6031f4/crates/ruff_python_ast/src/helpers.rs#L131-L134

We also have a more specific helper for traversing unions that may be helpful here:

https://github.com/astral-sh/ruff/blob/5c97b6ef40cd7fb34c2c69619657ef150d6031f4/crates/ruff_python_semantic/src/analyze/typing.rs#L453-L459

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:119 on 2026-01-16 22:03_

I could definitely be wrong, but I'm not sure we need to manually visit strings. String type annotations should already be classified as such and visited as type definitions:

https://github.com/astral-sh/ruff/blob/5c97b6ef40cd7fb34c2c69619657ef150d6031f4/crates/ruff_linter/src/checkers/ast/mod.rs#L2936

but we may need to adjust where the rule is applied. I.e. we may need to invoke `complex_annotation_function` not in `analyze::statement` but somewhere around here:

https://github.com/astral-sh/ruff/blob/5c97b6ef40cd7fb34c2c69619657ef150d6031f4/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L49-L62

I think these rules are somewhat similar to yours.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:192 on 2026-01-16 22:09_

If these are always static, we can use a `&'static str` in the rule struct, or even define an enum if there are a fixed number of options.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:57 on 2026-01-16 22:10_

Could you say what this trait is for? It's not really clear to me how useful it is, at least on a first pass through the code.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:283 on 2026-01-16 23:58_

```suggestion
        let flattened: Vec<_> = flatten_bin_op_expr(&expr.expr().as_bin_op_expr().unwrap())
```

---

_@ntBre reviewed on 2026-01-17 00:01_

Thanks for working on this! I had a few comments in addition to Amy's.

---

_Label `preview` added by @ntBre on 2026-01-17 00:01_

---

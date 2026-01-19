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
updated_at: 2026-01-18T23:38:51Z
url: https://github.com/astral-sh/ruff/pull/22427
synced_at: 2026-01-19T00:21:19Z
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

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:63 on 2026-01-16 21:57_

I think it would make sense to implement a `fix_title` function too (even if we can't provide an actual fix). That will attach a `help` sub-diagnostic to the end of the main diagnostic, where we could put a message like `consider using a type alias` or something. Just an idea.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:170 on 2026-01-16 21:59_

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

_@danjones1618 reviewed on 2026-01-18 21:13_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:40 on 2026-01-18 21:13_

Indeed, changing

---

_@danjones1618 reviewed on 2026-01-18 21:43_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:67 on 2026-01-18 21:43_

Found `match_typing_qualified_name` which allows me to simplify this section of the code! Thanks

---

_@danjones1618 reviewed on 2026-01-18 21:51_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:57 on 2026-01-18 21:51_

This is a small indirection to `sematnic.resolve_qualified_name` to allow for unit testing of `get_annotation_complexity` of individual annotation expressions without the need to build a checker over an entire python module.

I've refactored this trait to be simpler as I was originally expecting to need to handle `Union`s too and I've also found a helper function that already handles the  `typing` vs `typing_extensions` resolution.

---

_@danjones1618 reviewed on 2026-01-18 21:56_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:63 on 2026-01-18 21:56_

Nice idea!

---

_@danjones1618 reviewed on 2026-01-18 23:00_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:119 on 2026-01-18 23:00_

From a quick check, commenting out the `expr.as_string_literal_expr` results in tests failing.

Looking around in the code for expression, we could move the calls to calculate the annotation complexity into this function under a `checker.semantic.in_annotation()` guard. 

Regarding the self visiting of deferred string type definitions, this might work if we add the check function inside the aforementioned visitor function. Do you know if this will consider the entire annotation as deferred if a nested item is deferred such as `list[dict[str, "DeferredDefinition"]]`. Will this call at the `list` node or only from the string node?

---

_@danjones1618 reviewed on 2026-01-18 23:02_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:192 on 2026-01-18 23:02_

Unfortunately, this is the only location it's static as there's no argument name here. We could do a format string for it to become `<function_name>'s return type` if you think that's nicer?

---

_@danjones1618 reviewed on 2026-01-18 23:05_

---

_Review comment by @danjones1618 on `crates/ruff_linter/src/rules/flake8_annotation_complexity/rules/complex_annotation.rs`:170 on 2026-01-18 23:05_

Looks like the `traverse_union` could be a viable alternative - I can try refactor it later in the week

---

_Comment by @danjones1618 on 2026-01-18 23:07_

Thanks both @amyreese and @ntBre for the reviews. I've resolved your comments or responded to them. 

I can refactor to use the `traverse_union` and adjust the call sites of entering the check later in the week if they're still desired.

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 23:35_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+60365 -1 violations, +0 -0 fixes in 9 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+5680 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__main__.py#L15'>disnake/__main__.py:15:14:</a> TAE002 Type annotation for `entries` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__main__.py#L234'>disnake/__main__.py:234:27:</a> TAE002 Type annotation for `name` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__main__.py#L401'>disnake/__main__.py:401:21:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/_version.pyi#L11'>disnake/_version.pyi:11:19:</a> TAE002 Type annotation for `releaselevel` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L1043'>disnake/abc.py:1043:21:</a> TAE002 Type annotation for `base_attrs` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L1045'>disnake/abc.py:1045:15:</a> TAE002 Type annotation for `name` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L1046'>disnake/abc.py:1046:19:</a> TAE002 Type annotation for `category` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L1047'>disnake/abc.py:1047:21:</a> TAE002 Type annotation for `overwrites` is too complex (2 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L1048'>disnake/abc.py:1048:17:</a> TAE002 Type annotation for `reason` is too complex (1 > 0)
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/abc.py#L1052'>disnake/abc.py:1052:29:</a> TAE002 Type annotation for `overwrites_payload` is too complex (1 > 0)
... 5670 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+23183 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/docs/conf.py#L136'>airflow-core/docs/conf.py:136:51:</a> TAE002 Type annotation for `UTIL_MODULES_THAT_SHOULD_BE_INCLUDED_IN_API_DOCS` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/docs/conf.py#L140'>airflow-core/docs/conf.py:140:45:</a> TAE002 Type annotation for `MODELS_THAT_SHOULD_BE_INCLUDED_IN_API_DOCS` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/docs/conf.py#L146'>airflow-core/docs/conf.py:146:67:</a> TAE002 Type annotation for `exclude_patterns` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/docs/conf.py#L229'>airflow-core/docs/conf.py:229:21:</a> TAE002 Type annotation for `html_theme_options` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/docs/conf.py#L67'>airflow-core/docs/conf.py:67:19:</a> TAE002 Type annotation for `SYSTEM_TESTS_DIR` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/hatch_build.py#L49'>airflow-core/hatch_build.py:49:47:</a> TAE002 Type annotation for `versions` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/hatch_build.py#L60'>airflow-core/hatch_build.py:60:34:</a> TAE002 Type annotation for `return type` is too complex (2 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/__init__.py#L84'>airflow-core/src/airflow/__init__.py:84:17:</a> TAE002 Type annotation for `__lazy_imports` is too complex (2 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/client/local_client.py#L33'>airflow-core/src/airflow/api/client/local_client.py:33:44:</a> TAE002 Type annotation for `session` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/client/local_client.py#L46'>airflow-core/src/airflow/api/client/local_client.py:46:10:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/airflow_health.py#L29'>airflow-core/src/airflow/api/common/airflow_health.py:29:29:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/airflow_health.py#L37'>airflow-core/src/airflow/api/common/airflow_health.py:37:23:</a> TAE002 Type annotation for `triggerer_status` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/airflow_health.py#L38'>airflow-core/src/airflow/api/common/airflow_health.py:38:27:</a> TAE002 Type annotation for `dag_processor_status` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/mark_tasks.py#L108'>airflow-core/src/airflow/api/common/mark_tasks.py:108:15:</a> TAE002 Type annotation for `task_ids` is too complex (3 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/mark_tasks.py#L109'>airflow-core/src/airflow/api/common/mark_tasks.py:109:14:</a> TAE002 Type annotation for `run_ids` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/mark_tasks.py#L125'>airflow-core/src/airflow/api/common/mark_tasks.py:125:12:</a> TAE002 Type annotation for `tasks` is too complex (3 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/mark_tasks.py#L126'>airflow-core/src/airflow/api/common/mark_tasks.py:126:6:</a> TAE002 Type annotation for `return type` is too complex (3 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/mark_tasks.py#L212'>airflow-core/src/airflow/api/common/mark_tasks.py:212:13:</a> TAE002 Type annotation for `run_id` is too complex (1 > 0)
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/src/airflow/api/common/mark_tasks.py#L215'>airflow-core/src/airflow/api/common/mark_tasks.py:215:6:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
... 23164 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5715 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L106'>RELEASING/changelog.py:106:53:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L130'>RELEASING/changelog.py:130:61:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L160'>RELEASING/changelog.py:160:45:</a> TAE002 Type annotation for `labels` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L178'>RELEASING/changelog.py:178:20:</a> TAE002 Type annotation for `changelog` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L179'>RELEASING/changelog.py:179:18:</a> TAE002 Type annotation for `pr_info` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L231'>RELEASING/changelog.py:231:27:</a> TAE002 Type annotation for `return type` is too complex (2 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L253'>RELEASING/changelog.py:253:21:</a> TAE002 Type annotation for `` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L260'>RELEASING/changelog.py:260:23:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L266'>RELEASING/changelog.py:266:44:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/apache/superset/blob/f984dca5cc46c3badb2badcaa6b2893d5aa15d1d/RELEASING/changelog.py#L287'>RELEASING/changelog.py:287:28:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
... 5705 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2024 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L15'>examples/interaction/js_callbacks/js_on_event.py:15:41:</a> TAE002 Type annotation for `attributes` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L48'>examples/models/gauges.py:48:74:</a> TAE002 Type annotation for `direction` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/geo/tile_demo.py#L9'>examples/topics/geo/tile_demo.py:9:60:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L35'>release/action.py:35:47:</a> TAE002 Type annotation for `details` is too complex (2 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L134'>release/build.py:134:38:</a> TAE002 Type annotation for `content` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L137'>release/build.py:137:43:</a> TAE002 Type annotation for `content` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L144'>release/build.py:144:12:</a> TAE002 Type annotation for `files` is too complex (2 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/config.py#L37'>release/config.py:37:34:</a> TAE002 Type annotation for `` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/config.py#L38'>release/config.py:38:19:</a> TAE002 Type annotation for `` is too complex (1 > 0)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/config.py#L43'>release/config.py:43:24:</a> TAE002 Type annotation for `` is too complex (1 > 0)
... 2014 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+9818 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/cli.py#L65'>libs/cli/langchain_cli/cli.py:65:11:</a> TAE002 Type annotation for `port` is too complex (1 > 0)
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/cli.py#L69'>libs/cli/langchain_cli/cli.py:69:11:</a> TAE002 Type annotation for `host` is too complex (1 > 0)
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/dev_scripts.py#L14'>libs/cli/langchain_cli/dev_scripts.py:14:18:</a> TAE002 Type annotation for `config_keys` is too complex (1 > 0)
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/dev_scripts.py#L15'>libs/cli/langchain_cli/dev_scripts.py:15:22:</a> TAE002 Type annotation for `playground_type` is too complex (1 > 0)
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/namespaces/app.py#L129'>libs/cli/langchain_cli/namespaces/app.py:129:19:</a> TAE002 Type annotation for `dependencies` is too complex (2 > 0)
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/namespaces/app.py#L134'>libs/cli/langchain_cli/namespaces/app.py:134:15:</a> TAE002 Type annotation for `api_path` is too complex (2 > 0)
... 9812 additional changes omitted for rule TAE002
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/partners/qdrant/langchain_qdrant/vectorstores.py#L89'>libs/partners/qdrant/langchain_qdrant/vectorstores.py:89:9:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/partners/qdrant/langchain_qdrant/vectorstores.py#L89'>libs/partners/qdrant/langchain_qdrant/vectorstores.py:89:9:</a> DOC501 Raised exception `ValueError` missing from docstring
... 9811 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+4659 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/__init__.py#L348'>reflex/__init__.py:348:14:</a> TAE002 Type annotation for `_SUBMODULES` is too complex (1 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/admin.py#L18'>reflex/admin.py:18:12:</a> TAE002 Type annotation for `admin` is too complex (1 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1026'>reflex/app.py:1026:39:</a> TAE002 Type annotation for `app_wrappers` is too complex (2 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1032'>reflex/app.py:1032:45:</a> TAE002 Type annotation for `key` is too complex (1 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1109'>reflex/app.py:1109:49:</a> TAE002 Type annotation for `state` is too complex (2 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1189'>reflex/app.py:1189:23:</a> TAE002 Type annotation for `app_wrappers` is too complex (2 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1244'>reflex/app.py:1244:34:</a> TAE002 Type annotation for `performance_metrics` is too complex (2 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1285'>reflex/app.py:1285:26:</a> TAE002 Type annotation for `compile_results` is too complex (2 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1388'>reflex/app.py:1388:29:</a> TAE002 Type annotation for `modify_files_tasks` is too complex (3 > 0)
+ <a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L1391'>reflex/app.py:1391:29:</a> TAE002 Type annotation for `result_futures` is too complex (5 > 0)
... 4649 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+684 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/docs/conf.py#L181'>docs/conf.py:181:41:</a> TAE002 Type annotation for `info` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/docs/conf.py#L181'>docs/conf.py:181:60:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/docs/ext/conftabs.py#L16'>docs/ext/conftabs.py:16:22:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/docs/ext/conftabs.py#L86'>docs/ext/conftabs.py:86:24:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/noxfile.py#L68'>noxfile.py:68:19:</a> TAE002 Type annotation for `install_args` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/noxfile.py#L69'>noxfile.py:69:15:</a> TAE002 Type annotation for `run_args` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/noxfile.py#L70'>noxfile.py:70:13:</a> TAE002 Type annotation for `extras` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/src/scikit_build_core/__main__.py#L8'>src/scikit_build_core/__main__.py:8:18:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/src/scikit_build_core/_compat/__init__.py#L3'>src/scikit_build_core/_compat/__init__.py:3:10:</a> TAE002 Type annotation for `__all__` is too complex (1 > 0)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/41056e7b9aac3721994aa684de3314aa04c17dc9/src/scikit_build_core/_compat/builtins.py#L13'>src/scikit_build_core/_compat/builtins.py:13:18:</a> TAE002 Type annotation for `return type` is too complex (1 > 0)
... 674 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8057 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L110'>analytics/lib/counts.py:110:53:</a> TAE002 Type annotation for `output_table` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L120'>analytics/lib/counts.py:120:19:</a> TAE002 Type annotation for `interval` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L121'>analytics/lib/counts.py:121:23:</a> TAE002 Type annotation for `dependencies` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L130'>analytics/lib/counts.py:130:23:</a> TAE002 Type annotation for `output_table` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L131'>analytics/lib/counts.py:131:24:</a> TAE002 Type annotation for `pull_function` is too complex (2 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L143'>analytics/lib/counts.py:143:72:</a> TAE002 Type annotation for `realm` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L206'>analytics/lib/counts.py:206:49:</a> TAE002 Type annotation for `realm` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L235'>analytics/lib/counts.py:235:49:</a> TAE002 Type annotation for `realm` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L327'>analytics/lib/counts.py:327:30:</a> TAE002 Type annotation for `model_object_for_bucket` is too complex (1 > 0)
+ <a href='https://github.com/zulip/zulip/blob/cb4c27d1afdfcbe3bb7b3a8860097522f3a5c17b/analytics/lib/counts.py#L329'>analytics/lib/counts.py:329:15:</a> TAE002 Type annotation for `subgroup` is too complex (1 > 0)
... 8047 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TAE002 | 60364 | 60364 | 0 | 0 | 0 |
| DOC501 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>





---

_Comment by @danjones1618 on 2026-01-18 23:38_

Uh oh looks like a default value is missing. I'll fix tomorrow evening

---

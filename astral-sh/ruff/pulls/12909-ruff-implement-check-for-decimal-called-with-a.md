```yaml
number: 12909
title: "[ruff] Implement check for Decimal called with a float literal (RUF032)"
type: pull_request
state: merged
author: kbaskett248
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule/float-literal-decimal
created_at: 2024-08-15T18:50:55Z
updated_at: 2024-08-19T16:17:20Z
url: https://github.com/astral-sh/ruff/pull/12909
synced_at: 2026-01-12T15:55:42Z
```

# [ruff] Implement check for Decimal called with a float literal (RUF032)

---

_@kbaskett248_

## Summary

This PR adds a new check for a `Decimal` call with a float argument. As described in #11840, this is a problem when precision is important, which is usually why `Decimal` is used in the first place. To pull an example by @mayanyax from the issue:

```py
>>> Decimal(0.3) == Decimal(0.1) + Decimal(0.1) + Decimal(0.1)
False
>>> Decimal('0.3') == Decimal('0.1') + Decimal('0.1') + Decimal('0.1')
True
```
The check supports positive and negative floats. It also includes a fix that wraps the float in quotes.

## Test Plan

Test cases were added for the new rule.


---

_Comment by @github-actions[bot] on 2024-08-15 19:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+25 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L41'>tests/core/test_utils.py:41:29:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L49'>tests/core/test_utils.py:49:28:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L49'>tests/core/test_utils.py:49:52:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L49'>tests/core/test_utils.py:49:77:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L55'>tests/core/test_utils.py:55:52:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L55'>tests/core/test_utils.py:55:76:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L56'>tests/core/test_utils.py:56:40:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L71'>tests/core/test_utils.py:71:18:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L77'>tests/core/test_utils.py:77:19:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L77'>tests/core/test_utils.py:77:33:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L77'>tests/core/test_utils.py:77:48:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L80'>tests/core/test_utils.py:80:43:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L80'>tests/core/test_utils.py:80:57:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_utils.py#L80'>tests/core/test_utils.py:80:82:</a> RUF032 `Decimal()` called with float literal argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c88192c466cb91842310f82a61eaa48b39439bef/tests/providers/amazon/aws/transfers/test_dynamodb_to_s3.py#L94'>tests/providers/amazon/aws/transfers/test_dynamodb_to_s3.py:94:21:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/apache/airflow/blob/c88192c466cb91842310f82a61eaa48b39439bef/tests/providers/google/cloud/transfers/test_cassandra_to_gcs.py#L132'>tests/providers/google/cloud/transfers/test_cassandra_to_gcs.py:132:27:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/apache/airflow/blob/c88192c466cb91842310f82a61eaa48b39439bef/tests/www/views/test_views_trigger_dag.py#L104'>tests/www/views/test_views_trigger_dag.py:104:28:</a> RUF032 `Decimal()` called with float literal argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/9af3efec9f3e1df11fd34d93fdbfc89497f64706/ibis/expr/datatypes/tests/test_value.py#L48'>ibis/expr/datatypes/tests/test_value.py:48:26:</a> RUF032 `Decimal()` called with float literal argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/dtypes/cast/test_downcast.py#L36'>pandas/tests/dtypes/cast/test_downcast.py:36:39:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/dtypes/cast/test_downcast.py#L38'>pandas/tests/dtypes/cast/test_downcast.py:38:39:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/dtypes/test_missing.py#L326'>pandas/tests/dtypes/test_missing.py:326:21:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/tools/test_to_numeric.py#L195'>pandas/tests/tools/test_to_numeric.py:195:40:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/tools/test_to_numeric.py#L210'>pandas/tests/tools/test_to_numeric.py:210:31:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/tools/test_to_numeric.py#L210'>pandas/tests/tools/test_to_numeric.py:210:60:</a> RUF032 `Decimal()` called with float literal argument
+ <a href='https://github.com/pandas-dev/pandas/blob/a7a14108c2302c4dc01c65dce101fe2fd64ca7b9/pandas/tests/tools/test_to_numeric.py#L213'>pandas/tests/tools/test_to_numeric.py:213:37:</a> RUF032 `Decimal()` called with float literal argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF032 | 25 | 25 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-08-16 06:22_

---

_Label `preview` added by @MichaReiser on 2024-08-16 06:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:26 on 2024-08-16 06:28_

I find it difficult to understand what the rule `FloatLiteralDecimal` lints against because it's just a sequence of nouns.

How about `DecimalWithFloatValue`?

A more involved implementation could resolve the floats runtime value and compare if it is the same as the literal. This might even be a useful rule outside the context of `Decimal`. @AlexWaygood wdyt?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:12 on 2024-08-16 06:29_

Non-deterministic isn't the right word here. This means that the computer may use a different value of 0.3. That's not the case. The value for the 0.3 literal is deterministic. 

```suggestion
/// Float literals have limited precision that can lead to unexpected results. The `Decimal` class is designed to handle
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:70 on 2024-08-16 06:34_

Nit: We try to make some of the *cheap* checks first for better performance
```suggestion
    let Some(arg) = call.arguments.args.first() else {
    	return;
    }
    
    if !checker
        .semantic()
        .resolve_qualified_name(call.func.as_ref())
        .is_some_and(|qualified_name| matches!(qualified_name.segments(), ["decimal", "Decimal"]))
    {
        return;
    }
    
    if let ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
        value: ast::Number::Float(_),
        ..
    }) = arg
    {
        let diagnostic = Diagnostic::new(FloatLiteralDecimal, arg.range()).with_fix(
            fix_float_literal(arg.range(), &checker.generator().expr(arg)),
        );
        checker.diagnostics.push(diagnostic);
    } else if let ast::Expr::UnaryOp(ast::ExprUnaryOp { operand, .. }) = arg {
        if let ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
            value: ast::Number::Float(_),
            ..
        }) = operand.as_ref()
        {
            let diagnostic = Diagnostic::new(FloatLiteralDecimal, arg.range()).with_fix(
                fix_float_literal(arg.range(), &checker.generator().expr(arg)),
            );
            checker.diagnostics.push(diagnostic);
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:75 on 2024-08-16 06:35_

What's your reasoning for making it an unsafe edit. Is it because it changes the precision of the values and, therefore, could result in runtime changes?

---

_@MichaReiser approved on 2024-08-16 06:36_

Nice. I'd love to get some input from @AlexWaygood but this looks great

---

_@AlexWaygood reviewed on 2024-08-16 13:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:26 on 2024-08-16 13:29_

> I find it difficult to understand what the rule `FloatLiteralDecimal` lints against because it's just a sequence of nouns.
> 
> How about `DecimalWithFloatValue`?

I'd vote for `DecimalFromFloatLiteral` (since the `Decimal` object being constructed won't ultimately wrap a "float value" at all).

> A more involved implementation could resolve the floats runtime value and compare if it is the same as the literal. This might even be a useful rule outside the context of `Decimal`. @AlexWaygood wdyt?

For now I think I'd prefer to keep this a simple rule specific to `decimal.Decimal`, since this is a well-known footgun. We could maybe consider a more involved rule separately.

---

_@AlexWaygood reviewed on 2024-08-16 13:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:75 on 2024-08-16 13:32_

I think an unsafe edit is appropriate here... in most cases, you'll be changing the underlying value of the `Decimal` object being constructed. There might be code elsewhere that implicitly depended on the previous, "wrong" value that was caused by constructing the `Decimal` instance from the float, so it's entirely possible that fixing this error could unexpectedly cause your code to break.

@kbaskett248, it would be great if you could add one or two sentences to the docs explaining why this is an unsafe fix, [like how we do with other rules](https://docs.astral.sh/ruff/rules/verbose-raise/#fix-safety).

---

_@MichaReiser reviewed on 2024-08-16 13:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:26 on 2024-08-16 13:52_

The problem with considering a more involved rule separately is that the rules then have overlapping concerns which is something we generally want to avoid. That's why we may have to decide this now unless we're fine with redirecting later.

---

_@AlexWaygood reviewed on 2024-08-16 13:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:26 on 2024-08-16 13:56_

I'm still inclined to go for a specific rule that fixes a known problem where we can have a high degree of accuracy. It's unclear to me exactly how the more general rule would work and I doubt we'd have the same degree of accuracy trying to generalise it.

---

_@kbaskett248 reviewed on 2024-08-16 17:30_

---

_Review comment by @kbaskett248 on `crates/ruff_linter/src/rules/ruff/rules/float_literal_decimal.rs`:75 on 2024-08-16 17:30_

Honestly it was mostly unfamiliarity with the apis, but @AlexWaygood brings up a good point.

---

_@AlexWaygood reviewed on 2024-08-16 18:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/decimal_from_float_literal.rs`:27 on 2024-08-16 18:05_

```suggestion
/// constructed. This can lead to unexpected behavior if your program relies on the previous value
/// (whether deliberately or not).
```

---

_@AlexWaygood reviewed on 2024-08-16 18:37_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/decimal_from_float_literal.rs`:79 on 2024-08-16 18:37_

```suggestion
            is_arg_float_literal(operand.as_ref())
```

---

_Comment by @codspeed-hq[bot] on 2024-08-16 18:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/kbaskett248:rule/float-literal-decimal)

### Merging #12909 will **not alter performance**

<sub>Comparing <code>kbaskett248:rule/float-literal-decimal</code> (9a8fd54) with <code>main</code> (65de8f2)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @AlexWaygood on 2024-08-16 18:56_

> Merging #12909 will **degrade performances by 5.27%**

You can ignore that, it's been super flaky recently

---

_Comment by @kbaskett248 on 2024-08-16 19:18_

Thanks for your help with this @AlexWaygood and @MichaReiser! Any other updates you'd like me to make?

---

_@AlexWaygood reviewed on 2024-08-18 16:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/decimal_from_float_literal.rs`:81 on 2024-08-18 16:16_

One last thing. Here we should use the quote style that the user is generally using for the rest of their file, which won't necessarily be double quotes if they're not using autoformatting. We can do this with the `checker.stylist()`:

```suggestion
fn fix_float_literal(range: TextRange, float_literal: &str, stylist: &Stylist) -> Fix {
    let quote = stylist.quote();
    let content = format!("{quote}{float_literal}{quote}");
    Fix::unsafe_edit(Edit::range_replacement(content, range))
```

You'll just want to pass `checker.stylist()` into this function at the callsite, and you'll want to add `use ruff_python_codegen::Stylist;` to the top of the file so that you can use it here in the type signature

---

_Merged by @MichaReiser on 2024-08-19 09:22_

---

_Closed by @MichaReiser on 2024-08-19 09:22_

---

_Comment by @kbaskett248 on 2024-08-19 16:16_

Thank you both for your help getting this in!

---

_Comment by @AlexWaygood on 2024-08-19 16:17_

> Thank you both for your help getting this in!

Thanks for the great contribution!

---

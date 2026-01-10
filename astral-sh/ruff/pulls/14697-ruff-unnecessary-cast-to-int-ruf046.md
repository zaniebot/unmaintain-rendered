```yaml
number: 14697
title: "[`ruff`] Unnecessary cast to `int` (`RUF046`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF046
created_at: 2024-12-01T05:20:10Z
updated_at: 2024-12-07T14:02:15Z
url: https://github.com/astral-sh/ruff/pull/14697
synced_at: 2026-01-10T20:42:27Z
```

# [`ruff`] Unnecessary cast to `int` (`RUF046`)

---

_Pull request opened by @InSyncWithFoo on 2024-12-01 05:20_

## Summary

Resolves #11412.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @InSyncWithFoo on 2024-12-01 05:21_

Maybe this should be expanded to cover those suggested in #14292 as well, in which case the rule should be renamed to `unnecessary-cast-to-int`?

---

_Comment by @github-actions[bot] on 2024-12-01 05:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/loading.py#L86'>rasa/shared/core/training_data/loading.py:86:15:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/638f82b46d722f8131f53972643a2fdcc9d1aace/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L197'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:197:40:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/apache/superset/blob/638f82b46d722f8131f53972643a2fdcc9d1aace/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L199'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:199:29:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L107'>examples/server/app/spectrogram/main.py:107:13:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L338'>src/bokeh/colors/color.py:338:60:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1530'>src/bokeh/palettes.py:1530:27:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1568'>src/bokeh/palettes.py:1568:10:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1569'>src/bokeh/palettes.py:1569:10:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/f09be446890f81c4961efe4b3da35d7860099bee/indico/modules/rb/models/reservation_occurrences.py#L223'>indico/modules/rb/models/reservation_occurrences.py:223:24:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/4a55b100027fa3abe552b9527bc4bf260df39858/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L375'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:375:26:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/4a55b100027fa3abe552b9527bc4bf260df39858/astropy/io/fits/hdu/compressed/utils.py#L102'>astropy/io/fits/hdu/compressed/utils.py:102:13:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/4a55b100027fa3abe552b9527bc4bf260df39858/astropy/io/fits/tests/test_image.py#L1003'>astropy/io/fits/tests/test_image.py:1003:26:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/4a55b100027fa3abe552b9527bc4bf260df39858/astropy/io/votable/validator/html.py#L246'>astropy/io/votable/validator/html.py:246:14:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/4a55b100027fa3abe552b9527bc4bf260df39858/astropy/modeling/_fitting_parallel.py#L194'>astropy/modeling/_fitting_parallel.py:194:22:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/4a55b100027fa3abe552b9527bc4bf260df39858/astropy/utils/console.py#L365'>astropy/utils/console.py:365:21:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF046 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @sbrugman on 2024-12-01 11:20_

+1 unnecessary cast to int. This is more future proof: when type inference is available unnecessary int casts could be detected for a much wider class of functions.

Consider expanding this rule to other builtin functions such as “len”. Edit: indeed as suggested in the other issue.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round_cast.rs`:106 on 2024-12-02 08:33_

Nit: The 0 case is handled below by `find_argument("number", 0)?` and searching an empty `arguments` list is cheap

```suggestion
    if arguments.len() >2 {
        return None;
    }
```

---

_@MichaReiser reviewed on 2024-12-02 08:34_

I like your proposal to use the more generic name `unnecessary-cast-to-int`.

---

_Renamed from "[`ruff`] Unnecessary `round()` cast (`RUF046`)" to "[`ruff`] Unnecessary cast to `int` (`RUF046`)" by @InSyncWithFoo on 2024-12-02 15:29_

---

_Label `rule` added by @MichaReiser on 2024-12-02 17:48_

---

_Label `preview` added by @MichaReiser on 2024-12-02 17:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-02 17:49_

We should add an example for the cases where this is the case. I would expect that the rule is correct in almost all cases

---

_@MichaReiser reviewed on 2024-12-02 17:52_

---

_@InSyncWithFoo reviewed on 2024-12-02 17:56_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-02 17:56_

There has already been one:

```python
foo = type("Foo", (), {"__round__": lambda self: 4.2})()

int(round(foo))
```

---

_@MichaReiser reviewed on 2024-12-02 17:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-02 17:58_

That's good to know. 

We should extend the documentation and list all possible false positives and why they're false positives.

---

_@InSyncWithFoo reviewed on 2024-12-02 18:16_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-02 18:16_

Done.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-03 07:40_

Thanks

---

_@MichaReiser reviewed on 2024-12-03 07:40_

---

_@MichaReiser reviewed on 2024-12-03 07:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-03 07:52_

I'd probably rewrite this too, to make it sound less intimidating. 

```
When values incorrectly override the `__round__`, `__ceil__`, `__floor__`,
or `__trunc__` operators such that they don't return an integer,
this rule may produce false positives.
```

@AlexWaygood I'm leaning towards marking the fix as safe even in those cases. It's correct that this *could* alter behavior but the problem isn't the fix but the non-API compatible override of the said dunder methods.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:151 on 2024-12-03 07:54_

Nit: Rename to `create_round_edit`



```suggestion
fn create_round_edit(checker: &Checker, outer_range: TextRange, arguments: &Arguments) -> Option<Edit> {
    if arguments.len() > 2 {
        return None;
    }

    let number = arguments.find_argument("number", 0)?;
    let ndigits = arguments.find_argument("ndigits", 1);

    let number_expr = checker.locator().slice(number);
    let new_content = match ndigits {
        Some(Expr::NumberLiteral(ExprNumberLiteral { value, .. })) if is_literal_zero(value) => {
            format!("round({number_expr})")
        }
        Some(Expr::NoneLiteral(_)) | None => format!("round({number_expr})"),
        _ => return None,
    };

    Some(Edit::range_replacement(new_content, outer_range))
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:128 on 2024-12-03 07:56_

```suggestion
        Some(Expr::NumberLiteral(ExprNumberLiteral { value, .. })) if is_literal_zero(value) => {
            format!("round({number_expr})")
        }
```

This doesn't seem correct. `round` only returns an integer if `ndigits` is left unspecified or is `None`

```
>>> round(1.23, 0)
1.0
>>>
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF046.py`:36 on 2024-12-03 07:59_

This should not be an error

```
>>> round(0.1, 0)
0.0
```

---

_@MichaReiser requested changes on 2024-12-03 08:00_

Thanks. 

The special casing where `ndigitis=0` is incorrect. We should not flag that usage.

---

_@AlexWaygood reviewed on 2024-12-03 10:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-03 10:50_

Yes, if you've implemented the methods correctly, this should be safe. I don't think we need to worry too much here about edge cases where the user has implemented the dunder method in an incorrect way.

---

_Comment by @NeilGirdhar on 2024-12-03 19:16_

> when type inference is available unnecessary int casts could be detected for a much wider class of functions.

Just FYI, this may not be possible because Boolean values are (unfortunately) integers:
```python
def f(x: int):
  assert isinstance(x, int)
  print(x, int(x))  # If you remove `int` here, you change things.
f(True)
```

You could detect unnecessary casts, but you have to check that it's not used in any way that would matter, which is a lot trickier.

---

_Comment by @InSyncWithFoo on 2024-12-03 19:30_

@NeilGirdhar I made sure to take this into account: All functions listed there either delegate to their corresponding dunder methods or return a strict instance of `int`. If you can find one that doesn't, please let me know.

---

_Comment by @NeilGirdhar on 2024-12-03 19:58_

> @NeilGirdhar I made sure to take this into account: All functions listed there either delegate to their corresponding dunder methods or return a strict instance of `int`. If you can find one that doesn't, please let me know.

Yes, I wasn't talking about this excellent PR 😄 .  I was just responding to the comment that suggested that type inference can be used to discover unnecessary integer casts.

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-03 20:01_

Such overrides are not necessarily incorrect. [The documentation](https://docs.python.org/3/reference/datamodel.html#object.__round__) says “all these methods should return the value of the object truncated to an [`Integral`](https://docs.python.org/3/library/numbers.html#numbers.Integral) (typically an [`int`](https://docs.python.org/3/library/functions.html#int))”. They don’t need to return instances of `int` specifically, so calling `int` on the result might sometimes be necessary.

---

_@dscorbett reviewed on 2024-12-03 20:01_

---

_@InSyncWithFoo reviewed on 2024-12-03 20:34_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-03 20:34_

The explanation was changed as per [this comment](https://github.com/astral-sh/ruff/pull/14697#discussion_r1867203887). @MichaReiser @AlexWaygood I'm also a bit concerned about this myself, even though, to be fair, I don't know if there are enough such overrides in the wild to make false positives a problem.

---

_@MichaReiser reviewed on 2024-12-04 07:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-04 07:53_

Good point. I'm sorry. That's my fault. In that case my suggestion is to:

* Change the fix back to unsafe for the cases where this applies
* Add a fix safety section and explain for which method calls the fix is unsafe.

---

_@AlexWaygood reviewed on 2024-12-04 11:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:16 on 2024-12-04 11:22_

Yeah, agreed. Sorry from me too!

---

_@MichaReiser reviewed on 2024-12-05 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:139 on 2024-12-05 07:16_

Could we mark the fix as safe if `number_is_int` is `true`? 

---

_@MichaReiser approved on 2024-12-05 07:27_

This looks good to me. I pushed a few smaller nits, updated the documentation, and marked the fix for `round(2)` as safe. Let me know if that looks good to you and I'll merge

---

_Comment by @InSyncWithFoo on 2024-12-05 08:45_

@MichaReiser Looks good to me too. Let's get this merged.

---

_Merged by @MichaReiser on 2024-12-05 09:30_

---

_Closed by @MichaReiser on 2024-12-05 09:30_

---

_Branch deleted on 2024-12-05 12:27_

---

_Comment by @DanielYang59 on 2024-12-07 14:02_

I completely missed the party, but I'm amazed by how fast you respond, couldn't appreciate more

---

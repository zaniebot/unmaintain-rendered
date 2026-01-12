```yaml
number: 9728
title: "Implement RUF027: `Missing F-String Syntax` lint"
type: pull_request
state: merged
author: snowsignal
labels:
  - rule
assignees: []
merged: true
base: main
head: jane/linter/rule/fstring
created_at: 2024-01-31T03:24:54Z
updated_at: 2024-02-03T00:29:30Z
url: https://github.com/astral-sh/ruff/pull/9728
synced_at: 2026-01-12T15:55:30Z
```

# Implement RUF027: `Missing F-String Syntax` lint

---

_@snowsignal_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #8151

This PR implements a new rule, `RUF027`.

## What it does
Checks for strings that contain f-string syntax but are not f-strings.

### Why is this bad?
An f-string missing an `f` at the beginning won't format anything, and instead treat the interpolation syntax as literal.

### Example

```python
name = "Sarah"
dayofweek = "Tuesday"
msg = "Hello {name}! It is {dayofweek} today!"
```

It should instead be:
```python
name = "Sarah"
dayofweek = "Tuesday"
msg = f"Hello {name}! It is {dayofweek} today!"
```

## Heuristics
Since there are many possible string literals which contain syntax similar to f-strings yet are not intended to be,
this lint will disqualify any literal that satisfies any of the following conditions:
1. The string literal is a standalone expression. For example, a docstring.
2. The literal is part of a function call with keyword arguments that match at least one variable (for example: `format("Message: {value}", value = "Hello World")`)
3. The literal (or a parent expression of the literal) has a direct method call on it (for example: `"{value}".format(...)`)
4. The string has no `{...}` expression sections, or uses invalid f-string syntax.
5. The string references variables that are not in scope, or it doesn't capture variables at all.
6. Any format specifiers in the potential f-string are invalid.

## Test Plan

I created a new test file, `RUF027.py`, which is both an example of what the lint should catch and a way to test edge cases that may trigger false positives.

---

_Label `rule` added by @snowsignal on 2024-01-31 03:24_

---

_Review requested from @charliermarsh by @snowsignal on 2024-01-31 03:24_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-01-31 03:24_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-01-31 03:28_

This needs to be changed to accept the original string literal using `locator.slice(...)`

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:135 on 2024-01-31 03:29_

I'm working on making this more performant and avoiding memory allocations

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:39 on 2024-01-31 03:30_

The examples could probably be expanded. @charliermarsh would a description of the heuristics be useful, or is that too much?

---

_@snowsignal reviewed on 2024-01-31 03:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:104 on 2024-01-31 04:32_

We already rely on `memchr`, so you could consider using `memchr2` here to find all `{` and `}` characters, iterate over the matches, and ensure there's at least one of each? Though @BurntSushi would know if that will even be faster than what you have here :)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:114 on 2024-01-31 04:33_

I feel like I instinctively tend towards `Option<&[Keyword]>` for something like this. What are your preferences / tendencies?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:135 on 2024-01-31 04:35_

I believe this can be:

```rust
let kw_idents: FxHashSet<&str> = kwargs
    .map(|keywords| {
        keywords
            .iter()
            .filter_map(|k| k.arg.as_ref())
            .map(|arg| arg.as_str())
            .collect()
    })
    .unwrap_or_default();
```

And then below, `kw_idents.contains(id.as_str())`.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:146 on 2024-01-31 04:37_

Do you mind adding some comments here to guide the reader? E.g.:


---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:146 on 2024-01-31 04:37_

As a tip, you can use `semantic.resolve_name` instead of `semantic.lookup_symbol`, since you have an `ast::ExprName` here. (We save the binding that each name resolves to, so if you have a name rather than a string symbol, looking up whether its bound is really cheap.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:39 on 2024-01-31 04:38_

I think it'd make sense to include the heuristics as a standalone paragraph in the "Why is this bad?" , that's the pattern we tend to follow for other rules (e.g., to document when it does or does not trigger).

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:41 on 2024-01-31 04:39_

Nit (sorry): we tend to omit a period unless the message spans multiple sentences

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:55 on 2024-01-31 04:40_

Another option: check if `semantic.current_expression_parent().is_none()`, I think? Since that implies that this is a top-level expression.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:102 on 2024-01-31 04:42_

I think this can _maybe_ be simplified to: `Fix::unsafe_edit(Edit::insertion("f", literal.start()))`

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:101 on 2024-01-31 04:42_

Small tip: anything that implements `Ranged` gets a `.start()` and `.end()`, so these can be (e.g.) `literal.start()`.

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/ruff/RUF027.py`:74 on 2024-01-31 04:43_

What happens with: `"{a}".format(**kwargs)`?

I'm wondering if we should _always_ ignore strings that are the callee on a `.format` call. It seems like it would only ever be a false positive.

Similarly, what about `"{a}" % foo`, which is the older style of string formatting? We should _probably_ take care to ignore those too. What do you think?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:81 on 2024-01-31 04:44_

Should we iterate over _all_ parent expressions here? Or is it intentionally limited to the next two?

---

_@charliermarsh reviewed on 2024-01-31 04:44_

Excellent.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1332 on 2024-01-31 07:45_

Should this rule also trigger for implicit concatenated f-strings where one literal isn't an f-string literal but contains placeholders?

```python
output = (
	f"{a} + {b} = "
	"{a + b}"
)
```

it may not be worth supporting if it requires a lot of effort


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:94 on 2024-01-31 07:47_

Nit: I would make this helper local to your rule. I don't see it as something that comes up often and even if, duplicating this code won't hurt much. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:112 on 2024-01-31 07:49_

The same for this function. It seems to be fundamental to your rule, which is why I think it would be better to co-locate it with the rule rather than in the rule-agnostic `helpers` file (which would is unlikely to be the place where I would be looking for it)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:111 on 2024-01-31 07:50_

Nit: I had to read this comment multiple times to understand what it explains. Maybe add a small code example

```
/// ```python
/// def test(**kwargs):
/// 		... 
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:135 on 2024-01-31 07:54_

Cloning all `kwargs` for every string literal that is in a function with `kwargs` and collecting them to a hash set seems expensive. 

Assuming that it is rare to have a large number of `kwargs`: It might be faster to iterate over the `vec` in-place. It might actually be faster than doing a hash map lookup (because there are only few elements)


Edit: I just realized that this only applies to string literals containing `{}.` that's probably not too common, and, therefore, is less of a concern.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:143 on 2024-01-31 07:54_

Nit
```suggestion
        for element in f_string.elements.iter().filter_map(|element| element.as_expression()) {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:55 on 2024-01-31 07:59_

I think this checks something else... A comment explaining why we bail out for statement expressions where the value is a string literal would be helpful

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:56 on 2024-01-31 08:01_

Nit: 
```suggestion
        if value.is_string_literal() {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-01-31 08:04_

Can you explain why you would change it to use `locator.slice`? I fear that you could run into issue for parenthesized multline string literals.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:97 on 2024-01-31 08:05_

I think we need to be careful to not mess with escaped characters:

* The literal could be a raw-string (I recommend to not provide a fix if they contain any backslashes)
* We need to escape any `{` that have no matching `}`. (Okay. I think you're safe because of the `should_be_fstring`. The two are tightly coupled, which is why I think they should be co-located).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:101 on 2024-01-31 08:05_

Or you can use `range_replacement(content, literal)` 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF027_RUF027.py.snap`:2 on 2024-01-31 08:06_

I think it would be great to have some more examples with
* triple quoted strings (multiline and single line)
* single quoted, multiline strings
* Implicitly concatenated strings (if you want to have fun, with comments in between)
* string literals that use escaped characters (and `{` for something other than placeholders)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-01-31 08:09_

I wonder what happens if you have

```python
a = """b { # comment
c}  d
"""
```

when running in Python 3.12. I would assume that we accept this as a valid f-string :laughing: 

---

_@MichaReiser reviewed on 2024-01-31 08:09_

This is looking good. I mainly left a few nit comments and some suggestions for more tests (I fear, some of them will be painful)

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1332 on 2024-01-31 08:32_

I think it should be simple to add support for it but @snowsignal might have more idea here. Following changes would be required:

- Iterate over each parts of an `ExprFString` and calling the rule function on the `StringLiteral` 
- Update the rule calling function to take in `StringLiteral` (individual string) instead of the `StringLiteralValue` (concatenated version)
- Possibly move the rule calling logic to `string_like` then

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-01-31 08:38_

Can we avoid adding the quotes here and instead just add the prefix with the original string? I think that would require `locator.slice(...)` to extract to literal.

> I fear that you could run into issue for parenthesized multline string literals.

I'm not sure if that would be a problem here because, per my understanding, this function is called for individual string literal and not the concatenated one.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:55 on 2024-01-31 08:41_

As per the test cases, I believe this is checking if it's a standalone string literal and abort if so.

If that's the case and we decide to add support for string literals that are implicitly concatenated to a f-string, we might need to update this part to include `Expr::FString` as well.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:71 on 2024-01-31 08:43_

nit: maybe combine this using `with_fix`: `Diagnostic::new(...).with_fix(...)`

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:81 on 2024-01-31 08:54_

I want to understand the same. Is it possible that we capture unwanted keyword arguments (one test case suggests otherwise) or that we don't capture the necessary keyword arguments? An example would be helpful as well to better understand the reasoning behind this.

---

_@dhruvmanila reviewed on 2024-01-31 08:56_

---

_@snowsignal reviewed on 2024-01-31 10:37_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:114 on 2024-01-31 10:37_

I prefer `Option<&[Keyword]>` too. This was just an oversight, my bad.

---

_@snowsignal reviewed on 2024-01-31 10:38_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:104 on 2024-01-31 10:38_

Good suggestion!

---

_@snowsignal reviewed on 2024-01-31 10:41_

---

_Review comment by @snowsignal on `crates/ruff_linter/resources/test/fixtures/ruff/RUF027.py`:74 on 2024-01-31 10:41_

> I'm wondering if we should always ignore strings that are the callee on a .format call. It seems like it would only ever be a false positive.

I was considering adding something like that (since that's one of the things `pyanalyze` checks for). Let's do that.

> Similarly, what about "{a}" % foo, which is the older style of string formatting? We should probably take care to ignore those too. What do you think?

We definitely should, good call.

---

_@snowsignal reviewed on 2024-01-31 22:15_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1332 on 2024-01-31 22:15_

I think what @dhruvmanila suggested makes sense. I do think this is something we should support.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:104 on 2024-01-31 22:21_

Some ideas:

1. Since you're just looking for ASCII bytes, I would jetison the use of `string.chars()`. That's doing UTF-8 decoding when you don't need it because UTF-8 is ASCII compatible. So just `for &byte in possible_fstring.as_bytes() {` will do.
2. When both `opening` and `closing` are `true`, the loop can stop. Although I could see this hurting more than helping.
3. `memchr2` should have substantially higher throughput than what you have here, but its latency is _probably_ worse. By how much, I'm not sure. What that means is that if this functions is usually called with very short strings (I mean very short, like a few bytes), then your code here might beat `memchr2`. But if they're usually longer, then `memchr2` is likely about the best you can do. (And to be clear, even if `memchr2` is faster here, I don't have enough context here to guess at whether it will be a holistic improvement.)

---

_@BurntSushi reviewed on 2024-01-31 22:21_

---

_@snowsignal reviewed on 2024-01-31 22:50_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:146 on 2024-01-31 22:50_

This isn't working for me, and I wonder if it's because the binding id is different (since this `ExprName` came from a standalone `parse_expression` call)

---

_@snowsignal reviewed on 2024-01-31 23:15_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-01-31 23:15_

This works as a valid f-string in Python 3.12:
```
>>> a = f"""b { # comment
... c}  d
... """
>>> a
'b 4  d\n'
```

And yes, we catch this:
```
RUF027.py:37:22: RUF027 [*] `"""b { # comment
    c}  d
    """` may be a formatting string without an `f` prefix
   |
35 |       c = a
36 |       single_line = """ {a} """
37 |       multi_line = a = """b { # comment
   |  ______________________^
38 | |     c}  d
39 | |     """ 
   | |_______^ RUF027
   |
   = help: Add an `f` prefix to the formatting string

ℹ Unsafe fix
34 34 |     a = 4
35 35 |     c = a
36 36 |     single_line = """ {a} """
37    |-    multi_line = a = """b { # comment
   37 |+    multi_line = a = f"""b { # comment
38 38 |     c}  d
39 39 |     """ 
40 40 |     
```

Though we probably want to collapse the multi-line string for the error message somehow.

---

_Comment by @github-actions[bot] on 2024-01-31 23:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+91 -0 violations, +0 -0 fixes in 7 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/airflow/models/dag.py#L2155'>airflow/models/dag.py:2155:30:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/airflow/providers/amazon/aws/hooks/s3.py#L746'>airflow/providers/amazon/aws/hooks/s3.py:746:21:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/providers/docker/operators/test_docker.py#L566'>tests/providers/docker/operators/test_docker.py:566:28:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/test_utils/asserts.py#L94'>tests/test_utils/asserts.py:94:16:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/customjs_expr.py#L26'>examples/plotting/customjs_expr.py:26:49:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L47'>examples/styling/mathtext/latex_normal_distribution.py:47:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L48'>examples/styling/mathtext/latex_normal_distribution.py:48:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L49'>examples/styling/mathtext/latex_normal_distribution.py:49:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L50'>examples/styling/mathtext/latex_normal_distribution.py:50:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L51'>examples/styling/mathtext/latex_normal_distribution.py:51:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L52'>examples/styling/mathtext/latex_normal_distribution.py:52:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L53'>examples/styling/mathtext/latex_normal_distribution.py:53:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/legends.py#L581'>src/bokeh/models/annotations/legends.py:581:28:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7ce9e879681cbf04dd3747d59724756bfd9d3de3/ibis/backends/exasol/__init__.py#L70'>ibis/backends/exasol/__init__.py:70:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/ibis-project/ibis/blob/7ce9e879681cbf04dd3747d59724756bfd9d3de3/ibis/backends/snowflake/__init__.py#L338'>ibis/backends/snowflake/__init__.py:338:9:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/77188e0e04db70c7a1bb63008388791db37ed810/pandas/io/formats/format.py#L1266'>pandas/io/formats/format.py:1266:27:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/pandas-dev/pandas/blob/77188e0e04db70c7a1bb63008388791db37ed810/pandas/io/formats/format.py#L1268'>pandas/io/formats/format.py:1268:27:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/pandas-dev/pandas/blob/77188e0e04db70c7a1bb63008388791db37ed810/pandas/tests/dtypes/test_dtypes.py#L1063'>pandas/tests/dtypes/test_dtypes.py:1063:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/pandas-dev/pandas/blob/77188e0e04db70c7a1bb63008388791db37ed810/pandas/tests/indexes/base_class/test_formats.py#L138'>pandas/tests/indexes/base_class/test_formats.py:138:35:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/pandas-dev/pandas/blob/77188e0e04db70c7a1bb63008388791db37ed810/pandas/tests/indexes/base_class/test_formats.py#L141'>pandas/tests/indexes/base_class/test_formats.py:141:16:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/193be7f08964783f0b36d4b4011814f7ee29b342/rotkehlchen/tests/utils/blockchain.py#L410'>rotkehlchen/tests/utils/blockchain.py:410:38:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3ea800b719b1683380c1333c6788d0f7d7b50ef6/src/scikit_build_core/builder/wheel_tag.py#L84'>src/scikit_build_core/builder/wheel_tag.py:84:27:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+69 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/tools/droplets/create.py#L321'>tools/droplets/create.py:321:21:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/actions/streams.py#L1575'>zerver/actions/streams.py:1575:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/actions/streams.py#L1576'>zerver/actions/streams.py:1576:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/actions/streams.py#L1577'>zerver/actions/streams.py:1577:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/lib/redis_utils.py#L44'>zerver/lib/redis_utils.py:44:40:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/lib/typed_endpoint.py#L366'>zerver/lib/typed_endpoint.py:366:32:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/management/commands/edit_linkifiers.py#L14'>zerver/management/commands/edit_linkifiers.py:14:12:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/migrations/0423_fix_email_gateway_attachment_owner.py#L93'>zerver/migrations/0423_fix_email_gateway_attachment_owner.py:93:21:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/curl_param_value_generators.py#L281'>zerver/openapi/curl_param_value_generators.py:281:9:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1031'>zerver/openapi/python_examples.py:1031:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1046'>zerver/openapi/python_examples.py:1046:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1055'>zerver/openapi/python_examples.py:1055:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1096'>zerver/openapi/python_examples.py:1096:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1133'>zerver/openapi/python_examples.py:1133:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1157'>zerver/openapi/python_examples.py:1157:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1307'>zerver/openapi/python_examples.py:1307:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1398'>zerver/openapi/python_examples.py:1398:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1409'>zerver/openapi/python_examples.py:1409:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L1487'>zerver/openapi/python_examples.py:1487:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L303'>zerver/openapi/python_examples.py:303:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L315'>zerver/openapi/python_examples.py:315:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L321'>zerver/openapi/python_examples.py:321:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L333'>zerver/openapi/python_examples.py:333:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L343'>zerver/openapi/python_examples.py:343:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L355'>zerver/openapi/python_examples.py:355:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L362'>zerver/openapi/python_examples.py:362:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L379'>zerver/openapi/python_examples.py:379:17:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L454'>zerver/openapi/python_examples.py:454:25:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L469'>zerver/openapi/python_examples.py:469:25:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L477'>zerver/openapi/python_examples.py:477:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L572'>zerver/openapi/python_examples.py:572:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L588'>zerver/openapi/python_examples.py:588:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L627'>zerver/openapi/python_examples.py:627:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L796'>zerver/openapi/python_examples.py:796:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L809'>zerver/openapi/python_examples.py:809:17:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L934'>zerver/openapi/python_examples.py:934:45:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/a0e7f1296ff6d9445dc152b35cdc849aaf23c7b6/zerver/openapi/python_examples.py#L959'>zerver/openapi/python_examples.py:959:45:</a> RUF027 Possible f-string without an `f` prefix
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF027 | 91 | 91 | 0 | 0 | 0 |

</p>
</details>




---

_@snowsignal reviewed on 2024-01-31 23:28_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:81 on 2024-01-31 23:28_

This was intentional, though it's not clarified very well in the code itself.

>  Is it possible that we capture unwanted keyword arguments (one test case suggests otherwise) or that we don't capture the necessary keyword arguments?

The idea was to handle cases like `"{a}".format(a = 4)` where the parent expression is `Expr::Attribute` instead of `Expr::Call` - we have to go up one more layer for that.

The reason we only check the first two parents for a call with keyword arguments is because I didn't see a reason to go any higher.

---

_@snowsignal reviewed on 2024-01-31 23:29_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-01-31 23:29_

> I'm not sure if that would be a problem here because, per my understanding, this function is called for individual string literal and not the concatenated one.

Right, this is for individual string literals.

---

_@hauntsaninja reviewed on 2024-02-01 03:45_

---

_Review comment by @hauntsaninja on `crates/ruff_linter/resources/test/fixtures/ruff/RUF027.py`:74 on 2024-02-01 03:45_

The old-style printf formatting doesn't use curly braces anywhere (afaik), so shouldn't need it in this check

---

_@charliermarsh reviewed on 2024-02-01 04:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:146 on 2024-02-01 04:06_

Oh yes of course -- my bad. It has the be a node that we visited in the semantic model, and these are entirely new. Sorry!!!

---

_@dhruvmanila reviewed on 2024-02-01 05:08_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:122 on 2024-02-01 05:08_

> Though we probably want to collapse the multi-line string for the error message somehow.

I would be in favor of removing it from the message as it seems redundant with `--show-source`.

Might I suggest to use "f-string" instead of "formatting string" for consistency with other rule messages?

---

_Comment by @codspeed-hq[bot] on 2024-02-01 23:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/linter/rule/fstring)

### Merging #9728 will **not alter performance**

<sub>Comparing <code>jane/linter/rule/fstring</code> (3e5953e) with <code>main</code> (25d9305)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@snowsignal reviewed on 2024-02-02 21:01_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:81 on 2024-02-02 21:01_

Update: it turns out that the code for keyword matching did have to be a bit smarter. It now looks through all parent expressions, concatenating all keyword arguments, and also checks if a parent attribute expression contains a function call.

---

_Comment by @zanieb on 2024-02-02 21:13_

Really weird false positive

https://github.com/pandas-dev/pandas/blob/c3f7fee653fef5598055227821b6974f1eb87968/pandas/core/window/doc.py#L96-L115

They're basically doing `"{foo}".replace("{foo}", foo)` which such an anti-pattern that I'm not sure I mind?

---

_Comment by @zanieb on 2024-02-02 21:15_

Here's another false positive

https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/client.py#L2446

They're doing `data = {"foo": 1, "bar": 2}; "{foo} {bar}".format_map(data)` which seems like a true false positive. Perhaps we should ignore strings with a `.<call>` attached

---

_Comment by @zanieb on 2024-02-02 21:16_

This false positive is no good

https://github.com/apache/airflow/blob/3ec781946a7fcb5fa5bc99449d59d5981e6257ab/airflow/www/utils.py#L437-L446

e.g. `"{foo}".format(foo=1)` should definitely not be flagged

The vast majority of the false positives in the ecosystem are this.

I think we should also not flag `" = "{foo}"; x.format(foo=1)` if feasible.

---

_Comment by @zanieb on 2024-02-02 21:20_

This false positive is due to some inline JavaScript written as a string

https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/customjs_expr.py#L26-L38

I think that's okay...

---

_Comment by @snowsignal on 2024-02-02 21:22_

> e.g. "{foo}".format(foo=1) should definitely not be flagged

I think this ecosystem check might be outdated, this should be fixed.

---

_Comment by @zanieb on 2024-02-02 21:23_

> > e.g. "{foo}".format(foo=1) should definitely not be flagged
> 
> I think this ecosystem check might be outdated, this should be fixed.

Sweet. It'll update when the build succeeds.

---

_Comment by @snowsignal on 2024-02-02 21:24_

> Perhaps we should ignore strings with a `.<call>` attached

This is probably the best approach, honestly.

---

_Review requested from @charliermarsh by @snowsignal on 2024-02-02 21:41_

---

_Review requested from @zanieb by @snowsignal on 2024-02-02 21:58_

---

_Review requested from @MichaReiser by @snowsignal on 2024-02-02 21:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:92 on 2024-02-02 22:16_

Nit: extra dashes here.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:93 on 2024-02-02 22:16_

Does this need to be `pub`?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:57 on 2024-02-02 22:18_

We often do a weird thing here where we add a comment like:

```
/// RUF027
```

It just makes searching by code much easier (e.g., "Go to the definition of rule `RUF027`").

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:50 on 2024-02-02 22:20_

These show up in the editor, so I try to think of them in terms of how they'll appear in the Code Action menu:

![Screenshot 2024-02-02 at 5 19 20 PM](https://github.com/astral-sh/ruff/assets/1309177/dbdaf074-38f6-44d9-bc54-2affd8bb8f79)

So I might abbreviate this to: "Add `f` prefix" or "Add `f` prefix to f-string"


---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:34 on 2024-02-02 22:20_

Nit: this should say "Use instead:" for consistency with other documentation.

---

_@charliermarsh approved on 2024-02-02 22:21_

---

_Merged by @snowsignal on 2024-02-03 00:21_

---

_Closed by @snowsignal on 2024-02-03 00:21_

---

_Branch deleted on 2024-02-03 00:21_

---

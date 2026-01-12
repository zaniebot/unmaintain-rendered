```yaml
number: 7041
title: Add support for parsing f-string as per PEP 701
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - python312
assignees: []
merged: true
base: dhruv/pep-701
head: dhruv/fstring-parser
created_at: 2023-09-01T13:05:20Z
updated_at: 2023-09-14T02:09:12Z
url: https://github.com/astral-sh/ruff/pull/7041
synced_at: 2026-01-12T02:39:09Z
```

# Add support for parsing f-string as per PEP 701

---

_Pull request opened by @dhruvmanila on 2023-09-01 13:05_

## Summary

This PR adds support for PEP 701 in the parser to use the new tokens emitted by the lexer to construct the f-string node.

### Grammar

Without an official grammar, the f-strings were parsed manually. Now that we've the specification, that is being used in the LALRPOP to parse the f-strings.

### `string.rs`

This file includes the logic for parsing string literals and joining the implicit string concatenation. Now that we don't require parsing f-strings manually a lot of code involving the same is removed.

Earlier, there were 2 entry points to this module:
* `parse_string`: Used to parse a single string literal
* `parse_strings`: Used to parse strings which were implicitly concatenated

Now, there are 3 entry points:
* `parse_string_literal`: Renamed from `parse_string`
* `parse_fstring_middle`: Used to parse a `FStringMiddle` token which is basically a string literal without the quotes
* `concatenate_strings`: Renamed from `parse_strings` but now it takes the parsed nodes instead. So, we just need to concatenate them into a single node.

> A short primer on `FStringMiddle` token: This includes the portion of text inside the f-string that's not part of the expression and isn't an opening or closing brace. For example, in `f"foo {bar:.3f{x}} bar"`, the `foo `, `.3f` and ` bar` are `FStringMiddle` token content.

### `Constant::kind` changed in the AST

***Discussion in the official implementation: https://github.com/python/cpython/pull/102855#issuecomment-1508760963***

This change in the AST is when unicode strings (prefixed with `u`) and f-strings are used in an implicitly concatenated string value. For example,

```python
u"foo" f"{bar}" "baz" " some"
```

Pre Python 3.12, the kind field would be assigned only if the prefix was on the first string. So, taking the above example, both `"foo"` and `"baz some"` (implicit concatenation) would be given the `u` kind:

<details><summary>Pre 3.12 AST:</summary>
<p>

```python
Constant(value='foo', kind='u'),
FormattedValue(
  value=Name(id='bar', ctx=Load()),
  conversion=-1),
Constant(value='baz some', kind='u')
```

</p>
</details> 

But, post Python 3.12, only the string with the `u` prefix will be assigned the value:

<details><summary>Pre 3.12 AST:</summary>
<p>

```python
Constant(value='foo', kind='u'),
FormattedValue(
  value=Name(id='bar', ctx=Load()),
  conversion=-1),
Constant(value='baz some')
```

</p>
</details>

Here are some more iterations around the change:

1. `"foo" f"{bar}" u"baz" "no"`

<details><summary>Pre 3.12</summary>
<p>

```python
Constant(value='foo'),
FormattedValue(
  value=Name(id='bar', ctx=Load()),
  conversion=-1),
Constant(value='bazno')
```

</p>
</details>

<details><summary>3.12</summary>
<p>

```python
Constant(value='foo'),
FormattedValue(
  value=Name(id='bar', ctx=Load()),
  conversion=-1),
Constant(value='bazno', kind='u')
```

</p>
</details> 

2. `"foo" f"{bar}" "baz" u"no"`

<details><summary>Pre 3.12</summary>
<p>

```python
Constant(value='foo'),
FormattedValue(
  value=Name(id='bar', ctx=Load()),
  conversion=-1),
Constant(value='bazno')
```

</p>
</details>

<details><summary>3.12</summary>
<p>

```python
Constant(value='foo'),
FormattedValue(
  value=Name(id='bar', ctx=Load()),
  conversion=-1),
Constant(value='bazno')
```

</p>
</details>

3. `u"foo" f"bar {baz} realy" u"bar" "no"`

<details><summary>Pre 3.12</summary>
<p>

```python
Constant(value='foobar ', kind='u'),
FormattedValue(
  value=Name(id='baz', ctx=Load()),
  conversion=-1),
Constant(value=' realybarno', kind='u')
```

</p>
</details>

<details><summary>3.12</summary>
<p>

```python
Constant(value='foobar ', kind='u'),
FormattedValue(
  value=Name(id='baz', ctx=Load()),
  conversion=-1),
Constant(value=' realybarno')
```

</p>
</details> 

### Errors

With the hand written parser, we were able to provide better error messages in case of any errors such as the following but now they all are removed and in those cases an "unexpected token" error will be thrown by lalrpop:
* A closing delimiter was not opened properly
* An opening delimiter was not closed properly
* Empty expression not allowed

The "Too many nested expressions in an f-string" was removed and instead we can create a lint rule for that.

And, "The f-string expression cannot include the given character" was removed because f-strings now support those characters which are mainly same quotes as the outer ones, escape sequences, comments, etc.

## Test Plan

1. Refactor existing test cases to use `parse_suite` instead of `parse_fstrings` (doesn't exists anymore)
2. Additional test cases are added as required

Updated the snapshots. The change from `parse_fstrings` to `parse_suite` means that the snapshot would produce the module node instead of just a list of f-string parts. I've manually verified that the parts are still the same along with the node ranges.

## Benchmarks

https://github.com/astral-sh/ruff/pull/7263#issue-1889732755

fixes: #7043
fixes: #6835 

---

_Comment by @dhruvmanila on 2023-09-01 13:05_

Current dependencies on/for this PR:
* main
  * **PR #7035** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7035" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7013** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7013" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #6659** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6659" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7041** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7041" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
          * **PR #7211** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7211" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #7263** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7263" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #7331** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7331" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                  * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                      * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                        * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7041?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-09-01 13:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-parser)

### Merging #7041 will **degrade performances by 9.7%**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>dhruv/fstring-parser</code> (cd2e18b) with <code>main</code> (04183b0)</sub>



### Summary

`❌ 8` regressions
`✅ 17` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-parser)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/fstring-parser` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[numpy/globals.py]` | 233.2 µs | 252.9 µs | -7.8% |
| ❌ | `lexer[unicode/pypinyin.py]` | 620.2 µs | 673.4 µs | -7.89% |
| ❌ | `lexer[large/dataset.py]` | 9.8 ms | 10.7 ms | -8.42% |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.4 ms | 12.7 ms | -2.35% |
| ❌ | `lexer[numpy/ctypeslib.py]` | 2 ms | 2.1 ms | -7.65% |
| ❌ | `parser[large/dataset.py]` | 68.4 ms | 70.1 ms | -2.34% |
| ❌ | `lexer[pydantic/types.py]` | 4.1 ms | 4.6 ms | -9.7% |
| ❌ | `parser[unicode/pypinyin.py]` | 4.3 ms | 4.4 ms | -2.6% |


---

_Marked ready for review by @dhruvmanila on 2023-09-05 04:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-09-05 04:21_

---

_Review requested from @konstin by @dhruvmanila on 2023-09-05 04:21_

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:151 on 2023-09-05 07:09_

Unrelated to this PR but I stubmled over it when reviewing these changes: `Mode` still uses the wording `Jupyter`. Shouldn't it be `Mode::Ipython`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser.rs`:217 on 2023-09-05 07:13_

We probably want to start grouping `source`, `mode` and `source_path` by an abstraction but we don't have to do this as part of this PR. But we should probably do it before introducing more arguments.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:2096 on 2023-09-05 07:14_

Nit: Should this be part of the lexer changes?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1601 on 2023-09-05 07:20_

Nit: It wasn't clear to me how lalrpop would pass the location and `s` to `parse_fstring_middle` (an individual tuple? separate arguments?). That's why I think it is clearer to pass the arguments explicitly (which also allows to re-order the arguments)
```suggestion
    <location:@L> <s:fstring_middle> =>? Ok(parse_fstring_middle(fstring_middle, start_location)?),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1610 on 2023-09-05 07:23_

Can we rename `c` to `conversion`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1612 on 2023-09-05 07:25_


```suggestion
                format_spec_ref.map_or(end_location, |spec| spec.range().start()) - TextSize::from(1)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1622 on 2023-09-05 07:29_

Shouldn't `conversion` be initialized with the proper value even when no debug text is present? If not, group them in the grammar

`(<debug:"=" <c:FStringConversion?>)?`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1617 on 2023-09-05 07:32_

Nit: Very much personal preference but I find the many nested lambdas hard to read. Particularly because the map is also mutating its closure. 


```suggestion
            let end_offset = if let Some((conversion_start, conversion_flag)) = conversion {
                conversion = conversion_flag;
                conversion_start
            } else {
                // Subtract 1 to account for the closing brace or the `:` in the format spec
                format_spec_ref.map_or(end_location, |f| f.range().start()) - TextSize::from(1)
            }
            Some(ast::DebugText {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1635 on 2023-09-05 07:33_

Can you explain your reasoning for excluding the `:` from the format spec range?

---

_Label `accepted` added by @MichaReiser on 2023-09-05 07:36_

---

_Label `parser` added by @MichaReiser on 2023-09-05 07:36_

---

_Label `accepted` removed by @MichaReiser on 2023-09-05 07:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:255 on 2023-09-05 07:46_

It seems a bit strange that the function accepts a mixture of single arguments and tuples. I would change the signature to 

```
pub(crate) fn parse_fstring_middle(source: &str, is_raw: bool, start: TextSize) -> Result<Option<Expr>, LexicalError> {}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:158 on 2023-09-05 07:50_

The output string most likely has the same size as the input string. However, we aren't able to allocate a new string with the right capacity because we don't have the middle string here (okay, we kind of know that it is the source). 

Overall, the `parse_fstring_middle` feels strange to me and I'm inclined to instead inline this method into the public `parse_fstring_middle` function and use a `Cursor` for parsing. However, that would mean that you have to refactor `parse_escpaed_char` to use a `Cursor` internally and I don't know how much effort that is. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:163 on 2023-09-05 07:56_

This is unrelated to your change but `parse_escaped_char` should really not allocate a new string. I think it would be best to instead rename the method to `eat_escaped_char` and change it to accept a `Cursor` and a mutable string to which it appends the ouptut. 


In the future, we could go further and avoid allocating a new string alltogether if the input string (the plain fstring middle) and the "parsed" fstring middle matches. But let's not do this as part of this PR.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:244 on 2023-09-05 07:58_

The fact that this signature mixes arguments and tuple arguments is a bit strange. The alternative is to introduce a named `RawStringLiteral` type and pass that instead.


```suggestion
pub(crate) fn parse_string_literal(
    literal: &str,
    kind: StringKind,
    triple_quoted: bool,
    start: TextSize,
) -> Result<Expr, LexicalError> {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:297 on 2023-09-05 08:01_

Nit: this requires two full iterations. We can reduce it to one by using a plain loop

```rust
let mut has_fstring = false;
let mut byte_literal_count = 0u32;

for string in strings {
    has_fstring |= pexpr.is_f_string_expr();

    if is_bytes {
        byte_literal_count += 1;
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:298 on 2023-09-05 08:02_

What's the reason for passing `ParenthesizedExpr`? This should always either be StringLiteral, ByteLiteral, or FStringExpr nodes right? I recommend creating a new enum with just these three variants. It's very likely that that new union is smaller in size which helps performance. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:178 on 2023-09-05 08:04_

Lol, we should change kind to an enum. Allocating a string for this is silly (and I'm surprised the kind is a flag on constant when only strings can be unicode). But we can do this as a separate PR.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:280 on 2023-09-05 08:07_

Could you expand this comment with an example and why it is necessary? I know we talked about it on Discord but new and external maintainers won't know about the conversation or don't have access to it.

Overall: this is unfortunate because it requires moving all parts into a new Vec (flatten, then collect). I wonder if a dedicated token would be a better solution

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:338 on 2023-09-05 08:10_

I wonder if we should add a fast path (for the whole method) that handles the most common case of a single string:

* No need to test if it is a mixed byte / non-byte concatenation
* No need to copy the value (value.into_bytes() or use the original string)



---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:333 on 2023-09-05 08:15_

We should change this to a `String` and use `push_str` rather than building a vec first and than joining the parts. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:367 on 2023-09-05 08:16_

Should we change this to a `String` and use `push_str` directly instead of pushing parts and joining them at the end?

---

_@MichaReiser approved on 2023-09-05 08:18_

Nice work! Overall this is looking good to me. It will be interesting to get some ecosystem checks once ruff compiles. 

Most of my comments are nits but I think there's potential to clean up `string.rs` further (and hopefully improving performance at the same time). I leave it up to you if you want to do this as part of this or a follow up PR. 

---

_Review comment by @konstin on `crates/ruff_python_parser/src/lexer.rs`:2097 on 2023-09-05 09:06_

I tried to run `cargo run --bin ruff -- check --no-cache scratch.py` on file with `f"}"` inside but i got an infinitely memory hungry loop (776dd2a290487830013c036a28c21bb521146077)

---

_Review comment by @konstin on `crates/ruff_python_parser/src/parser.rs`:1285 on 2023-09-05 09:10_

implicit concatenation of different string types is such a weird feature

---

_Review comment by @konstin on `crates/ruff_python_parser/src/python.lalrpop`:1608 on 2023-09-05 09:19_

nit: `"{".text_len()` instead of `TextSize::from(1)`

---

_Review comment by @konstin on `crates/ruff_python_parser/src/string.rs`:2 on 2023-09-05 10:08_

nit:
```suggestion
//! Parsing of string literals, byte literals and implicit string concatenation.
```

---

_@konstin approved on 2023-09-05 10:17_

The infinite loop for something that succesfully fails in the unit test is strange

---

_@dhruvmanila reviewed on 2023-09-05 13:00_

---

_Review comment by @dhruvmanila on `crates/ruff/src/linter.rs`:151 on 2023-09-05 13:00_

My earlier thought process (which I'm quoting from #6395 PR description) was:

> The mode value is still `Mode::Jupyter` because the escape commands are part of the IPython syntax but the lexing/parsing is done for a Jupyter notebook.

But now that you've mentioned, I think `Mode::Ipython` makes more sense as we've all the nodes prefixed with `Ipy`.

---

_@dhruvmanila reviewed on 2023-09-05 13:11_

---

_Review comment by @dhruvmanila on `crates/ruff/src/linter.rs`:151 on 2023-09-05 13:11_

Opened #7153 :)

---

_@dhruvmanila reviewed on 2023-09-05 13:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:2096 on 2023-09-05 13:13_

Oh yes, I didn't notice these few tests earlier so I must've forgotten to switch the branch 😅 

---

_@dhruvmanila reviewed on 2023-09-05 13:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:1622 on 2023-09-05 13:29_

I'm unable to follow your comment with the highlighted code. The `None` is assigned to `debug_text` via the following condition which now that I'm looking at can be refactored using `debug.map(|_| {...})`:
```rust
        let debug_text = if debug.is_some() {
            let start_offset = location + TextSize::from(1);  // +1 to skip the opening brace
            let end_offset = if let Some((conversion_start, conversion_flag)) = fstring_conversion {
                conversion = conversion_flag;
                conversion_start
            } else {
                // Subtract 1 to account for the closing brace or the `:` in the format spec
                format_spec.as_ref().map_or(end_location, |spec| spec.range().start()) - TextSize::from(1)
            };
            Some(ast::DebugText {
                leading: source_code[TextRange::new(start_offset, value.range().start())].to_string(),
                trailing: source_code[TextRange::new(value.range().end(), end_offset)].to_string(),
            })
        } else {
            None
        };
```

---

_@dhruvmanila reviewed on 2023-09-05 13:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:1617 on 2023-09-05 13:30_

Oh yes, I do agree that this is more readable. Thanks!

---

_@dhruvmanila reviewed on 2023-09-05 13:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:1635 on 2023-09-05 13:34_

It isn't included in the official CPython implementation nor our earlier implementation.

https://play.ruff.rs/f1927a7d-bbb2-4afb-b56c-fffdaa74fa44

```
>>> mod.body[0].value.values[0].format_spec.values[0].col_offset
7
>>> mod.body[0].value.values[0].format_spec.values[0].end_col_offset
10
>>> s = 'f"{foo:.3f}"'
>>> s[7:10]
'.3f'
```

---

_@dhruvmanila reviewed on 2023-09-05 16:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:298 on 2023-09-05 16:57_

Yeah I actually thought of using that but it'll require moving the kind field (for unicode) into `StringConstant`. Giving a quick look at the usage of the kind field, it does not seem to be used many places so should be a quick refactor.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:178 on 2023-09-05 17:02_

I think it should be a boolean field `unicode` similar to `implicit_concatenated` on `StringConstant`. What do you think?

---

_@dhruvmanila reviewed on 2023-09-05 17:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1622 on 2023-09-06 07:59_

What I understand from the code:

* The `conversion` field is always `ConversionFlag::None` if `debug` is `None`
* But the grammar accepts `{test!s}` 

The grammer is correct in my view but the initialization of the `conversion` field is missing.

---

_@MichaReiser reviewed on 2023-09-06 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:1635 on 2023-09-06 08:01_

I would then lean towards moving the `:` into the `FStringReplacementField` to make it clear that the `:` is a separator and isn't part of the format spec node. 

The part I find difficult to understand is why is the `!` of the `FStringConversion` considered part of the node but the `:` is not. 

---

_@MichaReiser reviewed on 2023-09-06 08:01_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:1622 on 2023-09-06 12:23_

Oh right, that's a good find. I missed the initialization part. Thanks for noticing that!

---

_@dhruvmanila reviewed on 2023-09-06 12:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:1635 on 2023-09-06 12:25_

The `!` isn't considered part of any node. The reason for having the location before the `!` character is to extract out the start location of a `FStringConversion` which will be used to extract the debug text. Now, the downside for this is that an invalid conversion flag error would point to the `!` column instead of the actual value. We also can't directly add +1 to the location to point to the actual error location as there could be whitespace between `!` and the name token.

---

_@dhruvmanila reviewed on 2023-09-06 12:25_

---

_@dhruvmanila reviewed on 2023-09-06 16:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:367 on 2023-09-06 16:02_

Yes, we can then use `std::mem::take` to move the `String`.

---

_@dhruvmanila reviewed on 2023-09-06 16:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:1635 on 2023-09-06 16:42_

I'm using a similar pattern as `AssignSuffix`:

```
FStringFormatSpecSuffix = {
	":" <FStringFormatSpec>
}

FStringFormatSpec = {
	<location:@L> ...
}
```

---

_@dhruvmanila reviewed on 2023-09-06 16:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser.rs`:1285 on 2023-09-06 16:43_

Yeah, I'm still a bit on the edge with the last test case in this function :/

---

_@dhruvmanila reviewed on 2023-09-07 12:37_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:2097 on 2023-09-07 12:37_

This is a good find. The intention was not to check for infinite loop. The test function stops when it encounters an error but the lexer goes into an infinite loop when it's otherwise used.

---

_@dhruvmanila reviewed on 2023-09-08 03:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:298 on 2023-09-08 03:26_

PR: #7211 

---

_Label `python312` added by @dhruvmanila on 2023-09-08 03:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:2097 on 2023-09-09 14:33_

Also, note that `cargo run --bin ruff` won't work right now as the linter changes w.r.t. the new f-string tokens are yet to be implemented.

---

_@dhruvmanila reviewed on 2023-09-09 14:33_

---

_Comment by @hugovk on 2023-09-13 18:11_

Thanks for this PR!

I've tested it against CPython's f-string test file, and it runs as expected:

https://github.com/python/cpython/blob/3.12/Lib/test/test_fstring.py

Whereas `main` unsurprisingly fails to parse:

```
error: Failed to parse /Users/hugo/github/cpython/Lib/test/test_fstring.py:671:28: f-string: expecting '}'
/Users/hugo/github/cpython/Lib/test/test_fstring.py:671:28: E999 SyntaxError: f-string: expecting '}'
```

It would be great if this was ready by the time Python 3.12 is released on 2023-10-02 ([PEP 693](https://peps.python.org/pep-0693/)).

---

_Comment by @dhruvmanila on 2023-09-14 01:51_

> Thanks for this PR!

Hey, thanks for the comment. We're in the final phase of making the necessary changes across other parts of the codebase (linter and formatter) and then we'll merge this.

---

_Merged by @dhruvmanila on 2023-09-14 02:00_

---

_Closed by @dhruvmanila on 2023-09-14 02:00_

---

_Branch deleted on 2023-09-14 02:00_

---

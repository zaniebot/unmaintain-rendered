```yaml
number: 6167
title: "add `DebugText` for self-documenting f-strings"
type: pull_request
state: merged
author: davidszotten
labels:
  - parser
assignees: []
merged: true
base: main
head: self-documenting-f-strings
created_at: 2023-07-29T08:08:36Z
updated_at: 2023-08-01T05:55:04Z
url: https://github.com/astral-sh/ruff/pull/6167
synced_at: 2026-01-12T15:55:20Z
```

# add `DebugText` for self-documenting f-strings

---

_@davidszotten_

instead of modelling self-documenting f-strings (`f"{ foo= }"`) as a (simplified)
`Constant("foo=")` followed by a `FormattedValue(Expr("x"))`, instead model this case with a `DebugText(leading, trailing)` attribute on the `FormattedValue` so that we don't have to synthesize nodes (which results in siblings with overlapping ranges). We need to be able to preserve the whitespace for self-documenting f-strings, as well as reproduce the source (eg unparse, format).

Fixes #5970

## Test Plan

Existing snapshots, a few more tests. More needed?



---

_Comment by @github-actions[bot] on 2023-07-29 08:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.29ms     3.7 MB/sec    1.12     12.3±0.44ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.06ms     7.9 MB/sec    1.08      2.3±0.09ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    232.3±7.64µs    12.7 MB/sec    1.05   244.1±15.98µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.16ms     5.5 MB/sec    1.09      5.0±0.16ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.05     15.0±0.49ms     2.7 MB/sec    1.00     14.4±1.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.7±0.11ms     4.4 MB/sec    1.00      3.6±0.08ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   497.5±21.62µs     5.9 MB/sec    1.02   506.9±12.18µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.28ms     3.9 MB/sec    1.00      6.6±0.27ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.17      8.8±0.22ms     4.6 MB/sec    1.00      7.5±0.21ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.17  1797.2±47.89µs     9.3 MB/sec    1.00  1538.0±44.55µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.05    198.3±4.90µs    14.9 MB/sec    1.00    188.8±6.29µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.14      3.9±0.07ms     6.6 MB/sec    1.00      3.4±0.08ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     13.5±0.58ms     3.0 MB/sec    1.00     12.6±0.72ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.10      2.6±0.11ms     6.3 MB/sec    1.00      2.4±0.11ms     7.0 MB/sec
formatter/numpy/globals.py                 1.10   303.7±35.09µs     9.7 MB/sec    1.00   276.5±29.94µs    10.7 MB/sec
formatter/pydantic/types.py                1.16      6.1±0.39ms     4.2 MB/sec    1.00      5.2±0.22ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.14     20.5±1.08ms  2028.8 KB/sec    1.00     18.0±0.83ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.13      5.3±0.31ms     3.2 MB/sec    1.00      4.7±0.23ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.07   621.3±29.97µs     4.7 MB/sec    1.00   578.5±30.42µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.13      9.0±0.39ms     2.8 MB/sec    1.00      8.0±0.38ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.13     10.9±0.67ms     3.7 MB/sec    1.00      9.7±0.49ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07      2.1±0.10ms     7.8 MB/sec    1.00  1986.2±168.05µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.07   248.3±13.84µs    11.9 MB/sec    1.00   232.9±17.70µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.11      4.8±0.38ms     5.3 MB/sec    1.00      4.3±0.27ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @dhruvmanila by @MichaReiser on 2023-07-29 08:53_

---

_Label `parser` added by @MichaReiser on 2023-07-29 08:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1789 on 2023-07-29 08:54_

Would you mind adding a test case with a format specification?

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-29 08:55_

---

_@MichaReiser reviewed on 2023-07-29 08:55_

---

_@davidszotten reviewed on 2023-07-29 11:20_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_fstring_self_doc_trailing_space.snap`:34 on 2023-07-29 11:20_

note that we are removing the `Repr` setting here (implied by the self-doc)

---

_@davidszotten reviewed on 2023-07-29 11:22_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/string.rs`:321 on 2023-07-29 11:22_

this feels a little hacky but was my best idea. suggestions welcome

---

_@MichaReiser reviewed on 2023-07-29 20:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_fstring_self_doc_trailing_space.snap`:34 on 2023-07-29 20:07_

Could you explain your reasoning for removing it in more detail? 

---

_@davidszotten reviewed on 2023-07-30 06:55_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_fstring_self_doc_trailing_space.snap`:34 on 2023-07-30 06:55_

now that we are explicitly modelling self-documenting formatted values we can use `conversion` for _explicit conversions only (this one was implicit)

---

_@MichaReiser reviewed on 2023-07-31 10:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_fstring_self_doc_trailing_space.snap`:34 on 2023-07-31 10:12_

I think keeping `Repr` here is important. I understand that the default conversion for non-debug strings is `Conversion::Str`, but it is `Repr` for debug strings. I think it would be error-prone to not set `Conversion::Repr` because it requires all downstream code to correctly default to `Str` and `Repr` whether it is a debug string or not. 

My long-term preference would be to remove `Conversion::None` altogether and set it to the node's conversion. This makes the AST easier to use without having to remember conversions. This would require changing `unparse` to omit the `Conversion` when it is the default for that string type. I think that's fine because it means we only deal with the defaults at the input/output boundaries. I added it to the list of possible AST improvements https://github.com/astral-sh/ruff/discussions/6183#discussioncomment-6591993

---

_@davidszotten reviewed on 2023-07-31 10:21_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_fstring_self_doc_trailing_space.snap`:34 on 2023-07-31 10:21_

i guess we could change it for an `Option<ConversionFlag>` but surely we want to track any flags set in the source?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:595 on 2023-07-31 10:22_

Would it be possible to store a reference to a debug text instead of defining an extra `DebugText` node?
```suggestion
    debug_text: Option<&'a DebugText>,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:614 on 2023-07-31 10:22_

See comment above

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:929 on 2023-07-31 10:23_

```suggestion
#[derive(Clone, Debug, PartialEq, Eq, Hash)]
```

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1388 on 2023-07-31 10:24_

Nit: I find `Option<&ref>` more *natural* than passing a reference to an option (except if the option is mutable).

```suggestion
        debug_text: Option<&DebugText>,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:321 on 2023-07-31 10:31_

Which part do you find hacky?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:233 on 2023-07-31 10:32_

I like it that we now use the text ranges to get `leading` and `before_eq`. Would it be possible also to use text ranges to extract `trailing`? It would reduce 2 string allocations for each self-documenting string (one for `trailing_seq`, and one for formatting `trailing` in debug text.

---

_@MichaReiser approved on 2023-07-31 10:33_

This is great! 

I would like to hear your opinion on keeping `Conversion::Repr` for debug strings to ease downstream use and we may be able to speed this up a bit (the whole module requires a rework but we can start somewhere).

---

_Review comment by @dhruvmanila on `crates/ruff_python_codegen/src/generator.rs`:1789 on 2023-07-31 15:51_

If possible, an additional test case to mix both conversion and format specification would be nice as well: `f"{a=!r:0.05f}"`

---

_@dhruvmanila approved on 2023-07-31 16:25_

This is good! Thanks for working on this :)

---

_@davidszotten reviewed on 2023-07-31 20:47_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/string.rs`:233 on 2023-07-31 20:47_

i think so. we'd need to store the `=` and any trailing spaces inside `expression` which is a bit odd but doable if we just track where the actual expression ends. will push something with what i mean (and maybe you have ideas for how to improve that)

---

_@davidszotten reviewed on 2023-07-31 20:47_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/string.rs`:321 on 2023-07-31 20:47_

pulling slices out of `expression`

---

_@davidszotten reviewed on 2023-07-31 20:48_

---

_Review comment by @davidszotten on `crates/ruff_python_parser/src/string.rs`:328 on 2023-07-31 20:48_

Could we store references into the source here rather than allocating new strings? i guess many more things would need to change

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_fstring_self_doc_trailing_space.snap`:34 on 2023-08-01 05:50_

`Option<ConversionFlag>` is similar to what we have today:

* `Conversion::None` -> `None`
* `Conversion::Repr` -> `Some(ConversionFlags::Repr)`
* `Conversion::Str` -> `Some(ConversionFlags::Str)`

> i guess we could change it for an Option<ConversionFlag> but surely we want to track any flags set in the source?

Yeah, let's keep it the way it is. Knowing the conversion specified in the source would be helpful if we e.g. want to lint for unnecessary conversion specifiers. 

We could add a method on `FormattedValue` that returns the *used* conversion at runtime which always returns a none `None` value. 

---

_@MichaReiser reviewed on 2023-08-01 05:50_

---

_@MichaReiser reviewed on 2023-08-01 05:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:328 on 2023-08-01 05:53_

I played around with an ast that uses `Cow<'source, str>` for all string fields. I spent about 2h fixing lifetime issues and run out of patience. The problem is that every node must be parametrized by the source lifetime. 

We may still want to do this because using a bump allocator for the AST nodes would also require a new lifetime. Something to explore in the future.

---

_@MichaReiser reviewed on 2023-08-01 05:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:321 on 2023-08-01 05:54_

I think it's fine. I ultimately want to re-write this entire file and use `Cursor` and use string slices from the source text directly everywhere. But @dhruvmanila may make this whole code redundant before when implementing the 3.12 F-string changes.

---

_@MichaReiser approved on 2023-08-01 05:54_

---

_Merged by @MichaReiser on 2023-08-01 05:55_

---

_Closed by @MichaReiser on 2023-08-01 05:55_

---

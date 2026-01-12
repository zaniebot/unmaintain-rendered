```yaml
number: 5167
title: basic formatting for ExprDict
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-expr-dict
created_at: 2023-06-17T22:08:38Z
updated_at: 2023-07-07T20:48:21Z
url: https://github.com/astral-sh/ruff/pull/5167
synced_at: 2026-01-12T15:55:17Z
```

# basic formatting for ExprDict

---

_@davidszotten_

## Summary

basic formatting for ExprDict

## Test Plan

snapshots


---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__expression_py.snap`:690 on 2023-06-17 22:09_

this seems wrong, but not sure if it's an issue with dict formatting or the expressions for the values (or how those interact?)

---

_@davidszotten reviewed on 2023-06-17 22:09_

---

_Comment by @github-actions[bot] on 2023-06-17 22:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.8±0.01ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1371.5±10.82µs    12.1 MB/sec    1.00   1357.7±4.97µs    12.3 MB/sec
formatter/numpy/globals.py                 1.01    132.6±0.28µs    22.3 MB/sec    1.00    131.8±0.17µs    22.4 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.2 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.03ms     3.0 MB/sec    1.00     13.6±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.8 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    363.3±9.01µs     8.1 MB/sec    1.00    359.6±0.99µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.01      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1466.7±3.35µs    11.4 MB/sec    1.00   1469.6±3.14µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.4±0.20µs    18.7 MB/sec    1.00    157.8±0.19µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.43ms     4.1 MB/sec    1.00      9.9±0.43ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1983.3±80.85µs     8.4 MB/sec    1.02      2.0±0.12ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   205.7±13.57µs    14.3 MB/sec    1.04   214.4±18.15µs    13.8 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.20ms     6.2 MB/sec    1.00      4.0±0.15ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     20.3±0.81ms     2.0 MB/sec    1.00     20.2±0.79ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.25ms     3.2 MB/sec    1.00      5.2±0.21ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   642.4±27.91µs     4.6 MB/sec    1.00   640.4±34.31µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.08      9.3±0.49ms     2.7 MB/sec    1.00      8.7±0.29ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.32ms     4.0 MB/sec    1.00     10.2±0.54ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.2±0.11ms     7.4 MB/sec    1.00      2.2±0.12ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   267.5±17.57µs    11.0 MB/sec    1.00   267.8±20.70µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.8±0.22ms     5.4 MB/sec    1.00      4.7±0.21ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@davidszotten reviewed on 2023-06-18 08:31_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__expression_py.snap`:690 on 2023-06-18 08:31_

think i figured this out: need `group`

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:1 on 2023-06-18 11:28_

good test case

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-18 11:29_

Could you add a case for comments and `**`? something like
```python
{**a, #  leading
** # middle
b # trailing
}
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:87 on 2023-06-18 11:38_

it's an AST limitation but i'd nevertheless add a `debug_assert` that keys and values have the same length somewhere

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_compare.rs`:10 on 2023-06-18 11:38_

is this still needed?


---

_@konstin approved on 2023-06-18 11:39_

This already looks really good for such a complex node!

---

_@davidszotten reviewed on 2023-06-18 12:35_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:87 on 2023-06-18 12:35_

ah yes. intended to but forgot 👍 

---

_@davidszotten reviewed on 2023-06-18 12:36_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_compare.rs`:10 on 2023-06-18 12:36_

i guess not though it was handy. any reason not to keep it? (can make a separate commit if that's the issue)

---

_@davidszotten reviewed on 2023-06-18 12:49_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-18 12:49_

good idea

note that black (even for the first existing example) combines the comments more aggressively than i managed in this pr

me:

```
{
    # open
    key: value,  # key  # colon  # value
}  # close
```

black:

```
{key: value}  # open  # key  # colon  # value  # close
```

i couldn't figure out how to prevent e.g. the `# open` from being rendered when i printed the opening bracket

---

_@davidszotten reviewed on 2023-06-18 12:50_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:87 on 2023-06-18 12:50_

debug assert vs regular assert? (how do we choose?)

---

_@davidszotten reviewed on 2023-06-18 12:55_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-18 12:55_

ah, interesting. your example formats as 

```
{
    **a,  #  leading  # middle
    **b,  # trailing
}
```

which seem wrong (i would have attributed middle to b, not a.). is that a formatting bug in my pr or the parser assigning it to the "wrong" value?

---

_@konstin reviewed on 2023-06-18 12:56_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:87 on 2023-06-18 12:56_

normally `debug_assert!` in runtime code (for performance, and since we only use panics for things that should never fail) and `assert!` in tests

---

_@konstin reviewed on 2023-06-18 12:57_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_compare.rs`:10 on 2023-06-18 12:57_

it generates unnecessary code; I'm fine with adding the std derives on everything but we have kept only the necessary ones in the rest of the codebase and it should also be better for compile time

---

_Label `formatter` added by @konstin on 2023-06-18 13:35_

---

_@konstin reviewed on 2023-06-18 13:41_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-18 13:41_

you need to manually override this in `place_comments`. I tried to write some better docs for dangling comment in https://github.com/astral-sh/ruff/pull/5170, but switching leading and trailing is also done in `place_comments`.

---

_@davidszotten reviewed on 2023-06-18 17:18_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-18 17:18_

actually, was this a reply to ` # open  # key  # colon  # value  # close` or `# leading  # middle\n # trailing` ?

(and if different for the two, what about the other one)

---

_@konstin reviewed on 2023-06-18 20:14_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-18 20:14_

The formatting of `key: value` comments looks right. We don't currently match black for a bunch of comment placement and i don't know yet if we can sustain those deviations, but i want to go forward with this design.

For the `**b` case, you have to add a function to `place_comments` that uses the `SimpleTokenizer` or one of the wrapper functions to find `**` and assigns comments to nodes depending on whether they are left or right of the `**`. Searching `placements.rs` for "token" will show some reference code.

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:10 on 2023-06-19 08:21_

ok i got something working i think

---

_@davidszotten reviewed on 2023-06-19 08:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:932 on 2023-06-19 10:50_

Can we early return on any token other than `Star` (and `Comma`). Or are there any other tokens that may be in between the preceding node and the comment start? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:955 on 2023-06-19 10:54_

Is this to handle a `** # comment` for the last item? Can you try if this is already handled correctly by the default placement logic? I would expect that it is because `following` should always be `None` in that case.

If this handles another case, then it would be good to add a comment with a code sample of which case this branch handles.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:22 on 2023-06-19 10:58_

Nit: We try to avoid abbreviations in our code: 
```suggestion
struct KeyValuePair<'a> {
    key: &'a Option<Expr>,
    value: &'a Expr,
}
```

or `KeyValue`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:48 on 2023-06-19 10:59_

Nit: It could make sense to move the `group` out of the `KvPair` formatting and instead perform the grouping when formatting the items.

---

_@MichaReiser approved on 2023-06-19 11:00_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:919 on 2023-06-19 11:13_

`first_non_trivia_token_rev` might be easier

---

_@konstin reviewed on 2023-06-19 11:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:919 on 2023-06-19 11:23_

The forward version is prefered over `rev` (when possible) because rev always needs to find the start of the line to assert that the token isn't part of a comment.

```
# the slash is not a continuation token / 
# The following slash is a continuation token
a + /
```

---

_@MichaReiser reviewed on 2023-06-19 11:23_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:932 on 2023-06-19 11:52_

good q. will have a think

---

_@davidszotten reviewed on 2023-06-19 11:52_

---

_@davidszotten reviewed on 2023-06-19 11:56_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:955 on 2023-06-19 11:56_

all my test cases seemed to be handled correctly by the default behaviour for this. i suspect this is being overly cautious because i wasn't sure if that behaviour could be changing in the future. but i think i agree it's better to skip this for now

---

_@davidszotten reviewed on 2023-06-19 11:56_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:22 on 2023-06-19 11:56_

thanks. please err in favour of more nits, i like learning from them

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:48 on 2023-06-19 11:59_

1. could you explain _why you think it makes sense to do the grouping when formatting the items, that's not obvious to me (i don't have a preference but would like to understand the reason for yours)
2. the formatting happens inside `join_with`, how do i ask that to group the entries?

---

_@davidszotten reviewed on 2023-06-19 11:59_

---

_@davidszotten reviewed on 2023-06-19 12:23_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:932 on 2023-06-19 12:23_

actually do you think it would work if we shrink the `TextRange` (passed to the tokeniser) to 
`TextRange::new(preceding_end + TextSize::new(1), comment.slice().start()),` (to avoid the last char of the preceding token) and then just check that _all_ tokens are stars?

---

_@davidszotten reviewed on 2023-06-19 14:00_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:17 on 2023-06-19 14:00_

found a different issue i'm not sure how to approach:

```
{
    ** # between
    b,
}


{
    # before
    ** # between
    b,
}
```

formats as 

```
{
    **b,  # between
}


{
    **# before
    b,  # between
}
```

the `# before` is causes the stars to be (or stay?) split off again 

---

_@konstin reviewed on 2023-06-19 14:23_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:17 on 2023-06-19 14:23_

Could you try manually calling `leading_comments` to format `b`s leading comments before the `**`?

---

_@MichaReiser reviewed on 2023-06-19 15:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:48 on 2023-06-19 15:24_

Not a strong preference but it seems that we always want to group the whole item regardless of whether a key is present or not. Lifting the `group` out of the `KeyValuePair` formatting makes it clear that the grouping doesn't depend on the key's presence and it simplifies your formatting code (no need to use `format_args`). 

Hmm, yeah, using `group(&KeyValuePair { key, value })` probably won't work because of lifetimes. But the following should work

```rust
let joined = format_with(|f| {
	let mut joiner =  f.join_with(format_args!(text(","), soft_line_break_or_space()));

	for (key, value) in keys.iter().zip(values) {
		joiner.entry(&group(&KeyValuePair { key, value }));
	}

	joiner.finish()
});
```


---

_@davidszotten reviewed on 2023-06-19 18:54_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:17 on 2023-06-19 18:54_

seems to work. though i'm slightly confused about how that works. does `leading_comments()` (in addition to outputting the comment) also remove it from where it would otherwise automatically be added?

is there somewhere i can read about how the formatting system works?

---

_@davidszotten reviewed on 2023-06-19 19:26_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:48 on 2023-06-19 19:26_

ah, makes sense for always grouping 👍 

(though to handle the leading comment this is no longer the case i think)

---

_@MichaReiser reviewed on 2023-06-19 20:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/dict.py`:17 on 2023-06-19 20:28_

Yes, that's the case. Formatting comments marks them as *formatted* and they are then filtered out by the next `leading_node_comments` call. 

https://github.com/astral-sh/ruff/blob/48f4f2d63de7296d12b20893f452b876b2c64f76/crates/ruff_python_formatter/src/comments/format.rs#L51

There's no explicit documentation for it, but you can read the source. Formatting comments is also based on implementing `Format`, the same as implementing formatting for any node 

---

_@MichaReiser reviewed on 2023-06-19 20:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:932 on 2023-06-19 20:33_

The `end` of a range is always exclusive. For example, the range for the identifier `abc` in the following example is 5..8 (the 8th byte is not included). 

```python
a  = abc#comment
```

That means it should probably be safe to test if all tokens are `*` (or line continuation tokens, in case these are not handled as trivia and are allowed between the `**` )? 

Note: Simply adding `1` to the range would be unsafe if the last character of the preceding token is a non-ascii character with a representation that requires more than one byte (`TextRange` stores byte and not character ranges)

---

_@davidszotten reviewed on 2023-06-19 20:52_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:932 on 2023-06-19 20:52_

thanks

you can see the implementation i went with now (does it look ok?). i'm pretty sure the only characters we are skipping by adding 1 are `,` or `{` which are ascii

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:923 on 2023-06-19 21:05_

Using +1 may not be sufficient to skip the comma in case there's whitespace between the preceding node and the comma (or even a comment). I recommend to either call next on the iterator before the loop (with a debug assertion that it matches a comma or {), or matching on the kind in the loop and skipping over commas and {

---

_@MichaReiser approved on 2023-06-19 21:07_

Nice! 

---

_@davidszotten reviewed on 2023-06-19 21:29_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:923 on 2023-06-19 21:29_

nice catch. found an example like that which breaks this code. can fix by skipping first token instead of a single char 👍 

how come you suggest asserting the skipped token kind? (it can also be a colon which makes the list a bit tedious to check)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:923 on 2023-06-20 05:45_

My main motivation to add asserts if the list of possible kinds is limited (only a few kinds) is to catch incorrect ranges or wrong assumptions early. For example, I messed up the range in https://github.com/astral-sh/ruff/issues/5176. We wouldn't have know about this without the debug assertion being present. The debug assertions further act as documentation. They communicate to readers what kind of tokens are expected. 

You can use `matches!(token, Some(Token { kind: TokenKind::Colon | TokenKind::Comma | ... }))` which, hopefully, makes it less awkward. 

---

_@MichaReiser reviewed on 2023-06-20 05:45_

---

_Comment by @MichaReiser on 2023-06-20 09:18_

This is awesome. Thank you so much. 

---

_Merged by @MichaReiser on 2023-06-20 09:25_

---

_Closed by @MichaReiser on 2023-06-20 09:25_

---

_Comment by @davidszotten on 2023-06-20 10:02_

Thank you for the mentoring and for your patience with all my questions!

---

_Branch deleted on 2023-07-07 20:48_

---

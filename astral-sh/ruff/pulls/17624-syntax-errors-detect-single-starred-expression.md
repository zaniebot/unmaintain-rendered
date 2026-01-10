```yaml
number: 17624
title: "[syntax-errors] detect single starred expression assignment `x = *y`"
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - preview
assignees: []
merged: true
base: main
head: syntax-error
created_at: 2025-04-25T06:06:38Z
updated_at: 2025-05-01T17:40:58Z
url: https://github.com/astral-sh/ruff/pull/17624
synced_at: 2026-01-10T18:57:02Z
```

# [syntax-errors] detect single starred expression assignment `x = *y`

---

_Pull request opened by @abhijeetbodas2001 on 2025-04-25 06:06_

## Summary

Part of #17412

Starred expressions cannot be used as values in assignment expressions.
Add a new semantic syntax error to catch such instances.
Note that we already have `ParseErrorType::InvalidStarredExpressionUsage` to catch some starred expression errors during parsing, but that does not cover top level assignment expressions.

## Test Plan

- Added new inline tests for the new rule
- Found some examples marked as "valid" in existing tests (`_ = *data`), which are not really valid (per this new rule) and updated them
- There was an existing inline test - `assign_stmt_invalid_value_expr` which had instances of `*` expression which would be deemed invalid by this new rule. Converted these to tuples, so that they do not trigger this new rule.

---

_Review requested from @MichaReiser by @abhijeetbodas2001 on 2025-04-25 06:06_

---

_Review requested from @dhruvmanila by @abhijeetbodas2001 on 2025-04-25 06:06_

---

_Comment by @abhijeetbodas2001 on 2025-04-25 06:08_

@ntBre could you review?

---

_Comment by @github-actions[bot] on 2025-04-25 06:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
Failed to clone bokeh/bokeh: error: 3300 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-04-25 06:41_

---

_Assigned to @ntBre by @ntBre on 2025-04-25 16:21_

---

_@dhruvmanila reviewed on 2025-04-25 16:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-04-25 16:32_

Let's use the same convention for the message as other similar syntax errors: "Starred expression cannot be used here" (cc @ntBre)

---

_@ntBre reviewed on 2025-04-25 16:47_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-04-25 16:47_

Oh, that is actually what we used for the existing `InvalidStarExpression` variant, so that's my bad. I think the closest to the other variants would be something like "cannot use a starred expression here," but the current value is identical to CPython.

---

_@dhruvmanila reviewed on 2025-04-25 16:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-04-25 16:57_

I don't think we necessarily need to have the same message as CPython.

> I think the closest to the other variants would be something like "cannot use a starred expression here,"

Oh interesting. IIRC, the parser has messages like "Yield expressions cannot be used here", etc. which is where my recommendation comes from.

---

_@abhijeetbodas2001 reviewed on 2025-04-25 17:49_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-04-25 17:49_

> IIRC, the parser has messages like "Yield expressions cannot be used here"

There's also the exact message you mentioned above for star expressions:
 
https://github.com/astral-sh/ruff/blob/bc0a5aa4098a5b4d03365a22384b0307a124391c/crates/ruff_python_parser/src/error.rs#L229-L231

Should I change the syntax error to this message then?

---

_Label `preview` added by @MichaReiser on 2025-04-28 06:45_

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-04-28 06:45_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-04-28 07:18_

Hi, bumping this once again.
It is not clear to me whether the message needs to be changed or not.
(Although changing makes sense to me, happy to do that).

---

_@abhijeetbodas2001 reviewed on 2025-04-28 07:18_

---

_@dhruvmanila reviewed on 2025-04-28 12:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-04-28 12:51_

I'd recommend changing the message to "Starred expression cannot be used here" to be consistent with what the parser emits and other similar messages emitted by the Ruff parser. It'd be useful if that change could be done in a separate PR but if that's too much to ask, it's fine to include it in this PR itself.

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/err/assign_stmt_invalid_value_expr.py`:1 on 2025-04-28 18:19_

I think we can just leave this file alone. It doesn't look like the snapshots changed at all, so I don't think it's worth renaming the file or updating the tests, unless I'm missing something. They don't even trigger the new semantic error, right?

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/valid/statement/assignment.py`:41 on 2025-04-28 18:22_

I think removing the `*` would be enough to make this valid again. I'm guessing this test is more about testing that you can assign to `[]` and `()`, but @dhruvmanila would know better than me.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:111 on 2025-04-28 18:25_

Another couple of interesting `test_ok` cases would be:

```python
x = (*[1],)
x = *[1],
```

which is valid because the right-hand side is unpacked into a tuple.


---

_@ntBre reviewed on 2025-04-28 18:28_

Thanks, this is looking good. I think we can revert at least one of the snapshot updates and add a couple more interesting `test_ok` cases, but otherwise this looks good.

Let's follow up with a small PR updating the error message for this variant, as Dhruv suggested. I think it's okay to land this first since it's just reusing the current message.

---

_@dhruvmanila reviewed on 2025-04-28 18:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/valid/statement/assignment.py`:41 on 2025-04-28 18:38_

Yeah, probably. I don't really remember but looking at the file, I don't see any test case using starred expression on the RHS so I'll recommend to update it to the following instead:
```py
[] = (*data,)
() = (*data,)
```

---

_@abhijeetbodas2001 reviewed on 2025-04-29 05:34_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/resources/inline/err/assign_stmt_invalid_value_expr.py`:1 on 2025-04-29 05:34_

They do trigger the new rule.

```
        257 │+1 | x = *a and b
        258 │+  |     ^^^^^^^^ Syntax Error: can't use starred expression here
        259 │+2 | x = *yield x
        260 │+3 | x = *yield from x
        261 │+  |
        262 │+
        263 │+
        264 │+  |
        265 │+1 | x = *a and b
        266 │+2 | x = *yield x
        267 │+  |     ^^^^^^^^ Syntax Error: can't use starred expression here
        268 │+3 | x = *yield from x
        269 │+4 | x = *lambda x: x
```

I had removed the assignments so that they do not trigger it.

---

_Comment by @abhijeetbodas2001 on 2025-04-29 06:02_

Addressed two of the review comments, but Brent, please see the unresolved thread at https://github.com/astral-sh/ruff/pull/17624#discussion_r2065512771.

As you suggested, I'll make a follow up PR to edit the messages once this one is in.

---

_@dhruvmanila reviewed on 2025-04-29 20:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/inline/err/assign_stmt_invalid_value_expr.py`:1 on 2025-04-29 20:17_

I think that would be changing what the test was suppose to do. It's suppose to test that a starred expression involved in an assignment uses the correct precedence which is `bitwise_or` as per the grammar. We should retain this test but update it such that it still maintains what the original intent was. Now that the star expressions are not allowed at the top level, we can update the code to include the star expressions inside a list / tuple. For example:

```py
x = (*a and b,)
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1129 on 2025-04-29 20:19_

I'd recommend to keep using the "assign_stmt_" prefix as that's a good way to differentiate visually just by looking at the file list under the fixtures directory as to what statement this is testing as we don't have a way to categorize them because the files are autogenerated from these comments.

---

_@dhruvmanila reviewed on 2025-04-29 20:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:109 on 2025-04-29 20:20_

Let's use `assign_stmt_starred_expr_value` to have the "assign_stmt_" prefix.

---

_@dhruvmanila reviewed on 2025-04-29 20:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:115 on 2025-04-29 20:20_

Same here: Let's use `assign_stmt_starred_expr_value` to have the "assign_stmt_" prefix.

---

_@dhruvmanila reviewed on 2025-04-29 20:20_

---

_@ntBre reviewed on 2025-04-29 20:20_

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/err/assign_stmt_invalid_value_expr.py`:1 on 2025-04-29 20:20_

Ah, my mistake for not seeing that these would be affected. Thanks Dhruv for clarifying the intent of the tests, that looks like a great suggestion.

---

_@abhijeetbodas2001 reviewed on 2025-04-30 00:01_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/src/parser/statement.rs`:1129 on 2025-04-30 00:01_

Makes sense, done!

---

_@abhijeetbodas2001 reviewed on 2025-04-30 00:03_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/resources/inline/err/assign_stmt_invalid_value_expr.py`:1 on 2025-04-30 00:03_

Thanks for the explanation! Makes sense. For the non-`yield` test cases, I followed the pattern you mentioned, putting the expression inside a list.
For the `yield` test cases, I had to create a two-element tuple, because:
- Changing to `(*yield x,)` is not good enough, since this is still a "Starred expression" as a whole as per the AST
- Changing to `((*yield x),)` triggers another of the "Starred expression cannot be used here" errors, namely this one:
 https://github.com/astral-sh/ruff/blob/f11d9cb509fa823b9c52ff799db7077199472677/crates/ruff_python_parser/tests/snapshots/invalid_syntax%40expressions__parenthesized__parenthesized.py.snap#L62-L64

To avoid this being triggered, I went with a two-element tuple -- `(42, *yield from x)`

---

_Comment by @abhijeetbodas2001 on 2025-04-30 00:04_

Thanks a lot for the reviews! I have addressed all the comments and pushed.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/inline/err/assign_stmt_invalid_value_expr.py`:1 on 2025-04-30 15:24_

Thanks for following up, that works.

---

_@dhruvmanila reviewed on 2025-04-30 15:24_

---

_@dhruvmanila approved on 2025-04-30 15:25_

Looks good, I've mainly looked at the test cases. I'll leave the final review to @ntBre 

---

_@ntBre approved on 2025-04-30 19:03_

Looks great, thank you!

---

_Merged by @ntBre on 2025-04-30 19:04_

---

_Closed by @ntBre on 2025-04-30 19:04_

---

_Branch deleted on 2025-05-01 05:40_

---

_@abhijeetbodas2001 reviewed on 2025-05-01 17:40_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@single_starred_assignment_value.py.snap`:168 on 2025-05-01 17:40_

Opened #17772 for the message change.

---

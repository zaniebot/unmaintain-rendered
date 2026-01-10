```yaml
number: 10418
title: Update with item parsing to avoid infinite lookaheads
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/with-item-parsing
created_at: 2024-03-15T04:53:15Z
updated_at: 2024-03-20T17:11:16Z
url: https://github.com/astral-sh/ruff/pull/10418
synced_at: 2026-01-10T22:47:02Z
```

# Update with item parsing to avoid infinite lookaheads

---

_Pull request opened by @dhruvmanila on 2024-03-15 04:53_

## Summary

This PR updates the with item parsing logic for the new parser to avoid infinite lookaheads. It achieves this through speculative parsing without throwing away the parsed nodes.

The following are the steps taken by the parser:
1. If the parser encounters a `(`, take another path and assume that we're parsing a parenthesized with items
2. Parse the with items until the matching `)` is found or any token other than `,`
3. Update our assumption based on the current location of the parser and the parsed with items
4. If there are any more with items remaining, continue parsing them using list parsing with the deduced with item kind

## Review

The best way to review this would be to checkout the branch locally as the diff doesn't make it clear. There are comments throughout the with items parsing code which provides explanations with code examples.

## Test Plan

1. Add valid test cases
2. Add invalid test cases 


---

_Label `parser` added by @dhruvmanila on 2024-03-15 04:53_

---

_Comment by @github-actions[bot] on 2024-03-15 18:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (error)</summary>
<p>

```
Failed to pull: fatal: unable to access 'https://github.com/pypa/pip/': GnuTLS recv error (-54): Error in the pull function.
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
Failed to pull: fatal: unable to access 'https://github.com/pypa/cibuildwheel/': GnuTLS recv error (-54): Error in the pull function.
```

</p>
</details>




---

_Renamed from "WIP: With item parsing" to "Update with item parsing to avoid infinite lookaheads" by @dhruvmanila on 2024-03-18 04:38_

---

_Marked ready for review by @dhruvmanila on 2024-03-18 05:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-18 05:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1025 on 2024-03-19 09:03_

Nit: I found the "will raise the following error" comment confusing because it doesn't mention which error it raises (there's no following error). I assume *the following* refers to the "Expected an expression" error added below. 


You may want to rephrase the sentence to *while (2) will raise an expected an expression error*


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1029 on 2024-03-19 09:07_

What's the original assumption, and why is it problematic that a "fix" suggestion doesn't contradict the assumption? 

I think I have a basic understanding of the reasons but it might be helpful to explain in more detail.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1031 on 2024-03-19 09:08_

For (3), does the `parse_comma_seprated_list` generate the error for the trailing comma (because it first has to consume `, item3`)? If so, it might be worth to mentioning this because I was a bit confused about that 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1057 on 2024-03-19 09:10_

```suggestion
    /// parenthesized or if it's a parenthesized expression.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1056 on 2024-03-19 09:11_

Nit: It might be helpful to explain somewhere what an `ambiguous (` token is. What makes it ambiguous and what are the challenges.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1111 on 2024-03-19 09:15_

I believe this loop can get stuck (not progressing) when the parser isn't at a valid with item:

* the parser reached EOF
* the parser is at a `:` (with (a, b:`

That's why I think we need to exit the loop when we aren't positioned at the start of an expression

Edit: I see, the above cases work because we early exit if the parser isn't positioned at a comma after parsing the first item. Could we add a test for

```python
with (:
with(EOF
```

I think that works because `parse_with_items` calls into the expression parsing that will create empty expressions. But it might be better to avoid the empty expression and instead return an empty `vec`.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1135 on 2024-03-19 09:27_

Nit: I was a bit confused why we first set `has_trailing_comma` to false only to set it to `true` later

I would rewrite it to
```suggestion
            if !self.eat(TokenKind::Comma) {
                has_trailing_comma = false;
                break;
            }
            has_trailing_comma = true;
```

Or even

```suggestion
            has_trailing_comma = self.eat(TokenKind::Comma);
            if !has_trailing_comma {
                break;
            }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1167 on 2024-03-19 09:30_

Nit: It might be worth testing for `Indent` in case the colon is missing
```suggestion
            if matches!(self.peek(), TokenKind::Colon | TokenKind::Indent)  {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1182 on 2024-03-19 09:31_

```suggestion
                // For any other token followed by `)`, if any of the items has
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1220 on 2024-03-19 09:39_

Nit: It might be worth tracking this in a state variable instead of iterating over all items. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1100 on 2024-03-19 09:51_

Should we update the recovery context here to track that we're inside with item parsing to prevent that any inner parsing drops any tokens that belong to the with item headers:

```python
with (a, [b): pass
```

In this case, we would probably want to handle the `)` as a terminator (or even a `:`)


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1145 on 2024-03-19 09:54_

Can we skip this check if `with_item_kind` is `ParenthesizedExpression`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1249 on 2024-03-19 09:59_

Nit: I think you can call `parse_postfix_expression` directly. It returns `lhs` if the parser isn't positioned at a postfix token
```suggestion
            let context_expr = if self.parse_postfix_expression(lhs, start);
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1366 on 2024-03-19 10:09_

Nit: I think we can move the `!generator_expr.parenthesized` into the else branch
```suggestion
                        } else {
                            // For better error recovery. We would not take this path if the
                            // expression was parenthesized as it would be parsed as a generator
                            // expression by `parse_conditional_expression_or_higher`.
                            //
                            // ```python
                            // # This path will be taken for
                            // with (item, x for x in range(10)): ...
                            //
                            // # This path will not be taken for
                            // with (item, (x for x in range(10))): ...
                            // ```
                            let generator_expr = self.parse_generator_expression(parsed_expr.expr, start, false);
                            self.add_error(
                                ParseErrorType::OtherError(
                                    "unparenthesized generator expression cannot be used here"
                                        .to_string(),
                                ),
                                generator_expr.range(),
                            );
                            generator_expr
                        };
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:562 on 2024-03-19 10:14_

This error isn't intuitive because it is unclear why it would expect a Rpar, considering there's a trailing `)` at the end. It might be nice if we could improve it to be more specific (the generator needs to be parenthesized if it isn't the only element) -> We could verify the elements after parsing, for as long as parsing them doesn't result in any ambiguity.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:570 on 2024-03-19 10:17_

I don't think I understand why it expects a comma after `item`. Python errors after the first comma instead. 

Is it because we try to parse this as a tuple? But even adding a trailing comma doesn't produce valid syntax. I think we need to improve this error message to ensure it's a valid suggestion.

---

_@MichaReiser approved on 2024-03-19 10:25_

This is very impressive work. This is more involved than I expected and I love how you carefully added, and grouped the tests. I also like how you split the logic, e.g. I can read `parse_with_items` without being exposed to the full complexity of the `(` handling. 

I played a bit with the parsing to see if I can come up with cases that break the parser but I haven't found any case that it doesn't handle correctly, well done! 

The tests show two error messages that would be nice if we could improve them because they're somewhat misleading. 



---

_@dhruvmanila reviewed on 2024-03-20 12:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1029 on 2024-03-20 12:19_

The original assumption being that it's a parenthesized expression but we're suggesting to convert it into a parenthesized with items. We should raise an error based on our assumption because the _actual_ code could go either way.

I'll update the comment.

---

_@dhruvmanila reviewed on 2024-03-20 12:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1031 on 2024-03-20 12:20_

Yes, the list parsing generates the error. The `parse_with_items` will consume the comma (which happens right after this comment) and then continue to parse the remaining items with the list parsing.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1135 on 2024-03-20 12:29_

I like the second suggestion, thank you!

---

_@dhruvmanila reviewed on 2024-03-20 12:29_

---

_@dhruvmanila reviewed on 2024-03-20 12:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1145 on 2024-03-20 12:42_

Yeah, although it's going increase the nesting level by one more, it's already at 2 😆 

---

_@dhruvmanila reviewed on 2024-03-20 12:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1145 on 2024-03-20 12:46_

Oh never mind, I can merge the two `if` conditions so the nesting remains the same.

---

_@dhruvmanila reviewed on 2024-03-20 13:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1100 on 2024-03-20 13:15_

I'm not sure if this is required as we consider anything which isn't a `,` as a terminator.

---

_@dhruvmanila reviewed on 2024-03-20 14:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:562 on 2024-03-20 14:17_

This is happening because the generator expression parsing expects a `)` if it's considered to be inside the parenthesis which is the case here. The problem is that the parsed with item unconditionally sets the `used_ambiguous_lpar` to true but in this case the parser didn't _actually_ consume it. This means we break out of the parsing loop considering this to be a parenthesized expression.

One solution I can think of is to update the `parse_generator_expression` to accept an enum instead of `in_parentheses` bool which would be:

```rs
enum GeneratorExpressionParentheses { // or `GeneratorExpressionInParentheses`
	Yes, // `expect` parentheses
	No, // no parentheses
	Maybe, // for with item parsing, use `at` with `eat`
}
```

---

_@dhruvmanila reviewed on 2024-03-20 14:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:562 on 2024-03-20 14:17_

(I'll do this in a follow-up PR so that it's isolated and easy for you to review.)

---

_Comment by @codspeed-hq[bot] on 2024-03-20 14:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/with-item-parsing)

### Merging #10418 will **not alter performance**

<sub>Comparing <code>dhruv/with-item-parsing</code> (22119dc) with <code>dhruv/parser</code> (707d62a)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:562 on 2024-03-20 14:31_

And we need to make sure the range is correct in the `Maybe` case because it could or could not include the `(` location.

---

_@dhruvmanila reviewed on 2024-03-20 14:31_

---

_Comment by @dhruvmanila on 2024-03-20 14:58_

Uh-oh, need to look at the regression

---

_Comment by @dhruvmanila on 2024-03-20 15:39_

(Restarted the benchmark job since it seems like flakiness as it's back up in https://github.com/astral-sh/ruff/pull/10490#issuecomment-2009785770.)

---

_Comment by @dhruvmanila on 2024-03-20 15:40_

Oh wait, I need to rebase the parser branch 🤦‍♂️ 

---

_@MichaReiser reviewed on 2024-03-20 16:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:562 on 2024-03-20 16:13_

I see. Yeah, doing this in a separate PR seems fine. 

---

_@dhruvmanila reviewed on 2024-03-20 16:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:562 on 2024-03-20 16:19_

Fixed in https://github.com/astral-sh/ruff/pull/10490

---

_@dhruvmanila reviewed on 2024-03-20 16:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__ambiguous_lpar_with_items.py.snap`:570 on 2024-03-20 16:19_

Fixed in https://github.com/astral-sh/ruff/pull/10490

---

_Merged by @dhruvmanila on 2024-03-20 17:11_

---

_Closed by @dhruvmanila on 2024-03-20 17:11_

---

_Branch deleted on 2024-03-20 17:11_

---

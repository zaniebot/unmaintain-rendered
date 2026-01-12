```yaml
number: 11055
title: "Add `ExpressionContext` for expression parsing"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/expr-context
created_at: 2024-04-20T09:43:28Z
updated_at: 2024-04-23T04:29:49Z
url: https://github.com/astral-sh/ruff/pull/11055
synced_at: 2026-01-12T15:55:34Z
```

# Add `ExpressionContext` for expression parsing

---

_@dhruvmanila_

## Summary

This PR adds a new `ExpressionContext` struct which is used in expression parsing.

This solves the following problem:
1. Allowing starred expression with different precedence
2. Allowing yield expression in certain context
3. Remove ambiguity with `in` keyword when parsing a `for ... in` statement

For context, (1) was solved by adding `parse_star_expression_list` and `parse_star_expression_or_higher` in #10623, (2) was solved by by adding `parse_yield_expression_or_else` in #10809, and (3) was fixed in #11009. All of the mentioned functions have been removed in favor of the context flags.

As mentioned in #11009, an ideal solution would be to implement an expression context which is what this PR implements. This is passed around as function parameter and the call stack is used to automatically reset the context.

### Recovery

How should the parser recover if the target expression is invalid when an expression can consume the `in` keyword?

1. Should the `in` keyword be part of the target expression?
2. Or, should the expression parsing stop as soon as `in` keyword is encountered, no matter the expression?

For example:
```python
for yield x in y: ...

# Here, should this be parsed as
for (yield x) in (y): ...
# Or
for (yield x in y): ...
# where the `in iter` part is missing
```

Or, for binary expression parsing:
```python
for x or y in z: ...

# Should this be parsed as
for (x or y) in z: ...
# Or
for (x or y in z): ...
# where the `in iter` part is missing
```

This need not be solved now, but is very easy to change. For context this PR does the following:
* For binary, comparison, and unary expressions, stop at `in`
* For lambda, yield expressions, consume the `in`

## Test Plan

1. Add test cases for the `for ... in` statement and verify the snapshots
2. Make sure the existing test suite pass
3. Run the fuzzer for around 3000 generated source code
4. Run the updated logic on a dozen or so open source repositories (codename "parser-checkouts")


---

_Label `internal` added by @dhruvmanila on 2024-04-20 09:43_

---

_Label `parser` added by @dhruvmanila on 2024-04-20 09:43_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-20 09:43_

---

_Comment by @github-actions[bot] on 2024-04-20 10:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-04-20 14:14_

I'll review on Monday but a few ahead of time questions

For recovery, I assume the recovery is unchanged or did you change the recover?

> For binary, comparison, and unary expressions, stop at in

I think the whole point of disallowing in is to disallow ambiguity in this case. For example, you could have `for a in b + c + d + e in 4:` which isn't valid but detecting this would be more involved. That's why I think hard stopping for binary expression is the right call. 

---

_@dhruvmanila reviewed on 2024-04-21 05:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__named__invalid_target.py.snap`:201 on 2024-04-21 05:26_

> For recovery, I assume the recovery is unchanged or did you change the recover?

The recovery for `star_named_expression` got better as a side effect. Why better? Because a named expression is actually allowed here, but it's the target expression which is invalid.

---

_Comment by @dhruvmanila on 2024-04-21 05:31_

> For recovery, I assume the recovery is unchanged or did you change the recover?

Actually, it's changed in one specific case:

```python
for yield x in y: ...
for lambda x, y: x in y: ...
```

The previous logic would hard stop at `in` keyword here. It would actually hard stop in every expression. But now, this PR doesn't pass in the context in `yield` and `lambda` expression and thus the `in` is part of the `yield` and `lambda` expression now.

Before this PR:
```
Syntax Error: Yield expression cannot be used here at byte range 4..11
  |
1 | for yield x in y: ...
  |     ^^^^^^^ Syntax Error: Yield expression cannot be used here
  |
```

This PR:
```
Syntax Error: Yield expression cannot be used here at byte range 4..16
  |
1 | for yield x in y: ...
  |     ^^^^^^^^^^^^ Syntax Error: Yield expression cannot be used here
  |


Syntax Error: Expected 'in', found ':' at byte range 16..17
  |
1 | for yield x in y: ...
  |                 ^ Syntax Error: Expected 'in', found ':'
  |
```

I think we should change this to retain the behavior in `yield` expression and is also the same for Biome's JS parser ([playground](https://biomejs.dev/playground/?code=eQBpAGUAbABkACAAeAAgAGkAbgAgAHkAOwAKAGYAbwByACAAKAB5AGkAZQBsAGQAIAB4ACAAaQBuACAAeQApACAAewB9AA%3D%3D)).

For `lambda` expression, I think the new behavior is correct.

---

_Comment by @MichaReiser on 2024-04-22 07:19_

> I think we should change this to retain the behavior in yield expression and is also the same for Biome's JS parser ([playground](https://biomejs.dev/playground/?code=eQBpAGUAbABkACAAeAAgAGkAbgAgAHkAOwAKAGYAbwByACAAKAB5AGkAZQBsAGQAIAB4ACAAaQBuACAAeQApACAAewB9AA%3D%3D)).

By retain you mean retaining the existing or the new behavior? 

I think I would find it more correct if the yield would stop before the `in` but I don't feel strongly, especially if it complicates the logic.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:111 on 2024-04-22 07:21_

Nit: I would move this to the end of the file, considering that it is an implementation detail (what I'm most interested when jumping to this file is to see how expression parsing works)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:128 on 2024-04-22 07:21_

Nit: You could change it to `EXCLUDE_IN` so that you can remove the `default` implementation

Nit: I think it should otherwise be `ALLOW_IN` to align with the other flags.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:157 on 2024-04-22 07:24_

I think this method is incorrect in the sense that it only adds the `ALLOW_STARRED_EXPRESSION` flags but never removes them. We should either rename the method or ensure to correctly unset the flags (at least the `BITWISE_OR_PRECEDENCE`)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:137 on 2024-04-22 07:26_

Nit: The name here doesn't really fit with the other flags and it seems specific to `star` expression but the name doesn't say so. 

`STARRED_BITWISE_OR_PRECEDENCE`, `ALLOW_STARRED_CONDITIONAL_PRECEDENCE` or `DISALLOW_STARRED_CONDITIONAL_PRECEDENCE`

---

_Comment by @dhruvmanila on 2024-04-22 07:32_

> By retain you mean retaining the existing or the new behavior?

Retaining the existing behavior i.e., stopping before the `in` keyword.

I don't think it _should_ complicate the logic as it just involves passing the context and set / unset the flags.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:267 on 2024-04-22 07:33_

Nit: The context seems to always be `default` except when called from `parse_named_expression_or_higher` or the list parsing. 

Should we split the method into a `parse_conditional_expression_or_higher` that defaults to `ExpressionContext::default` and have a `parse_conditional_expression_or_higher_impl` that has an optional `ExpressionContext`? It could help making writing new expression parsing easier if the default context is what should be used in all places where `parse_conditional_expression_or_higher` is used (I don't need to think that hard about what context I need to pass)

---

_Comment by @dhruvmanila on 2024-04-22 07:34_

That said, I won't be making that change in this PR. I'll do it as a follow-up to avoid mixing things up. You can review this PR in the current state.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1920 on 2024-04-22 07:36_

Nit: You could offer a helper to create the context: `ExpressionContext::starred_allowed(...)`.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1915 on 2024-04-22 07:37_

There's a difference between the new and old parsing in that the previous implementation didn't unset the `INCLUDE_IN` flag because it was stored on the parser context. 

I think that's correct, considering that we're inside of a list. Did you verify that all call sites that now create a new context can omit setting the `INCLUDE_IN`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2264 on 2024-04-22 07:39_

I'm not sure if we should inherit the context here, to avoid unsetting the `INCLUDE_IN`


```suggestion
                    context
                        .with_starred_expression_allowed(StarredExpressionPrecedence::BitwiseOr),
                )
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2333 on 2024-04-22 07:39_

Should we inherit the context here to avoid `for a:= in b` to eat the `in`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1110 on 2024-04-22 07:44_

Nit: I would inline `context` similar to how you do it in other call-sites.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1199 on 2024-04-22 07:45_

Nit: I would probably have kept `parse_star_expression_list` to avoid the repetition of the context creation (or have a helper that creates the star expression context)

---

_@MichaReiser approved on 2024-04-22 07:47_

---

_@dhruvmanila reviewed on 2024-04-23 02:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:157 on 2024-04-23 02:47_

Good catch! I'll change it to remove the flag in the `StarredExpressionPrecedence::Conditional` case and use a `match` expression to make it explicit in case any new variant is added in the future.

---

_@dhruvmanila reviewed on 2024-04-23 02:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:137 on 2024-04-23 02:59_

Yeah, I agree. I'm renaming it to `STARRED_BITWISE_OR_PRECEDENCE` because, by default, starred expression would be at `Conditional` precedence.

---

_@dhruvmanila reviewed on 2024-04-23 03:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:267 on 2024-04-23 03:09_

Yeah, I didn't realize that. I just counted them and it seems like so:

  | default | non-default | inherit
-- | -- | -- | --
expression list | 6 | 11 |  
named expression | 5 | 11 |  
conditional expression | 19 | 3 | 1


---

_@dhruvmanila reviewed on 2024-04-23 03:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:702 on 2024-04-23 03:24_

@MichaReiser we unset the `FOR_TARGET` flag here which allowed the `in` keyword to be part of a binary expression. For example, `[a, b in c, d]`

---

_@dhruvmanila reviewed on 2024-04-23 03:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1915 on 2024-04-23 03:26_

We did unset the `FOR_TARGET` flag which is equivalent to setting the `INCLUDE_IN`. Here, the default context sets the `INCLUDE_IN` flag. Refer: https://github.com/astral-sh/ruff/pull/11055/files#r1575593474

---

_@dhruvmanila reviewed on 2024-04-23 03:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:2264 on 2024-04-23 03:29_

I'm assuming this is specific to the yield expression for which I commented on the PR. If so, I'll create a follow-up to pass in the context to yield expressions in a later PR. I think it makes sense to avoid consuming the `in` keyword in a yield expression.

---

_@dhruvmanila reviewed on 2024-04-23 03:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:2333 on 2024-04-23 03:31_

I don't think that's required at this point because the target value of a `for` statement uses the `expressions` (`parse_expression_list`) grammar rule which will stop at `:=` token as it doesn't parse a named expression. Maybe something to lookout for when working on error recovery.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1199 on 2024-04-23 03:32_

Yeah, I'm thinking of going through the `ExpressionContext` creation sites and creating methods for the most common case(s).

---

_@dhruvmanila reviewed on 2024-04-23 03:32_

---

_@dhruvmanila reviewed on 2024-04-23 03:52_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:128 on 2024-04-23 03:52_

I like the `EXCLUDE_IN` which is what we actually want to set. Thanks for the suggestion!

---

_@dhruvmanila reviewed on 2024-04-23 04:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1199 on 2024-04-23 04:10_

invocations | count
-- | --
starred, conditional | 5
starred, bitwise | 11
starred, bitwise, yield | 7
others | ...

Based on the above table, I'm adding the following methods:
* `ExpressionContext::starred_conditional`
* `ExpressionContext::starred_bitwise_or`
* `ExpressionContext::yield_or_starred_bitwise_or`

---

_Comment by @codspeed-hq[bot] on 2024-04-23 04:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/expr-context)

### Merging #11055 will **improve performances by 6.64%**

<sub>Comparing <code>dhruv/expr-context</code> (d980e27) with <code>main</code> (62478c3)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/expr-context` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 5.4 ms | 5.1 ms | +6.64% |
| ⚡ | `parser[pydantic/types.py]` | 11.4 ms | 10.9 ms | +4.53% |


---

_Merged by @dhruvmanila on 2024-04-23 04:19_

---

_Closed by @dhruvmanila on 2024-04-23 04:19_

---

_Branch deleted on 2024-04-23 04:19_

---

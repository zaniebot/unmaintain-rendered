```yaml
number: 10828
title: Add tests for parameters
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/parameters
created_at: 2024-04-08T03:51:34Z
updated_at: 2024-04-09T16:11:33Z
url: https://github.com/astral-sh/ruff/pull/10828
synced_at: 2026-01-10T22:37:01Z
```

# Add tests for parameters

---

_Pull request opened by @dhruvmanila on 2024-04-08 03:51_

## Summary

This PR adds test cases for parameters in a function definition.

---

_Label `parser` added by @dhruvmanila on 2024-04-08 03:51_

---

_Label `testing` added by @dhruvmanila on 2024-04-08 03:51_

---

_Marked ready for review by @dhruvmanila on 2024-04-08 12:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-08 12:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:261 on 2024-04-08 13:45_

Most other diagnostics have the *cannt* or *expected* where this states what happened:
```suggestion
                    "Parameter without a default cannot follow a parameter with a default"
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2685 on 2024-04-08 13:47_

Nit: I think I would move the method to `Parser` similar to the expression validation function so that you can push the errors directly with `self.add_error`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:310 on 2024-04-08 13:49_

I don't think I understand this comment. When is it possible that we parse a node that is to the right of the next token?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2345 on 2024-04-08 13:52_

I wonder if it still makes sense to have a single function or if it should rather be a `parse_function_parameter` and `parse_lambda_parameter` function because there's not much code that is shared between the two kinds (most of the code is different). Or is that not possible because there's a single `parse_parameters` function that calls into `parse_parameter`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2495 on 2024-04-08 13:56_

Could we use 

```
let param_star_range = self.node_range(star_start)
```

instead? 

I'm cautious of `cover` because it can extend the range in unintended ways if we aren't careful.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2550 on 2024-04-08 13:57_

Same here: Can we use use `Parser::node_range` to get the range of the `double_star` and param range

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2459 on 2024-04-09 07:18_

It would help if you can rephare the "same problem". It's unclear what it refers to. Is it arguments parsing?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2504 on 2024-04-09 07:24_

Nit and unrelated to this PR. You could try to add `#[cold]` to `add_error` and profile if it speeds up the "happy path" where we have no parse errors.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2600 on 2024-04-09 07:51_

Is it intentional that this test case is commented out?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2623 on 2024-04-09 07:52_

What happens if there are two slashes? Does it then swap the parameters twice? 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2641 on 2024-04-09 07:54_

For the future: We may also want to handle keywords here `def test(async=true): pass` for better error recovery

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__lambda_default_parameters.py.snap`:93 on 2024-04-09 07:55_

I think this is fixed now?

---

_@MichaReiser approved on 2024-04-09 07:58_

---

_@dhruvmanila reviewed on 2024-04-09 08:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:310 on 2024-04-09 08:05_

In binary expression, the parser would start by parsing the LHS. then depending on the operator, recurse and parse the RHS.

But, the problem isn't just for binary expressions. It's for the first node where the problem is. Looking the the provided example, the `last_token_end` isn't part of the node that we're parsing right now. In this scenario, if the node is missing, we'd use the `missing_node_range` which creates an empty range at the last token end position. But, the current expression hasn't started yet, so this missing node range is going to be outside the parsed expression.

---

_@dhruvmanila reviewed on 2024-04-09 08:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2345 on 2024-04-09 08:07_

It should be possible because of the `FunctionKind`. We can then call either `parse_function_parameter` or `parse_lambda_parameter` depending on the enum variant.

---

_@dhruvmanila reviewed on 2024-04-09 08:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2623 on 2024-04-09 08:15_

Oh, this is a good point. This would then swap the parameters again which would create problem in the AST validation as the positional only parameters are visited first.

In this case, for better error recovery, I think we should extend the existing positional only arguments.

So, for the happy path where `/` is only once, we'd swap it but for any subsequent `/`, we'd extend it if there are any arguments.

---

_@MichaReiser reviewed on 2024-04-09 08:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2345 on 2024-04-09 08:30_

Okay, yeah but if it's used in there than it probably doesn't make that much sense. I think leaving it as is is fine.

---

_Comment by @codspeed-hq[bot] on 2024-04-09 14:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/parameters)

### Merging #10828 will **degrade performances by 8.96%**

<sub>Comparing <code>dhruv/parameters</code> (246426a) with <code>dhruv/parser</code> (8917847)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/parameters)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/parameters` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.74% |
| ❌ | `parser[numpy/ctypeslib.py]` | 5 ms | 5.4 ms | -8.96% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.4% |


---

_Comment by @github-actions[bot] on 2024-04-09 14:38_

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

_@dhruvmanila reviewed on 2024-04-09 15:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2623 on 2024-04-09 15:31_

Hmm, no, we'd need to drop the token. For example,

```py
def foo(a, /, b, c, /): ...
```

Here, only `a` should be considered as positional-only argument while `b` and `c` are regular arguments otherwise the order would be incorrect. This should be the case at least with the current AST.

---

_@dhruvmanila reviewed on 2024-04-09 15:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2600 on 2024-04-09 15:51_

Yes, it turned out to be an actual bug as you pointed out in the following comment (https://github.com/astral-sh/ruff/pull/10828#discussion_r1557145352) that's fixed.

---

_Merged by @dhruvmanila on 2024-04-09 15:53_

---

_Closed by @dhruvmanila on 2024-04-09 15:53_

---

_Branch deleted on 2024-04-09 15:53_

---

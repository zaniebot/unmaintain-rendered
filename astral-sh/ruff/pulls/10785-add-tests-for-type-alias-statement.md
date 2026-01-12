```yaml
number: 10785
title: Add tests for type alias statement
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/type-params
created_at: 2024-04-05T04:20:30Z
updated_at: 2024-04-09T09:11:50Z
url: https://github.com/astral-sh/ruff/pull/10785
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for type alias statement

---

_@dhruvmanila_

## Summary

This PR adds test cases for a type alias statement.

This doesn't really contain a lot of test cases because for most of them the lexer converts the `type` keyword to a `Name` token. They can be added when we remove the soft keyword transformer and move that logic in the parser.



---

_Label `parser` added by @dhruvmanila on 2024-04-05 04:20_

---

_Label `testing` added by @dhruvmanila on 2024-04-05 04:20_

---

_@dhruvmanila reviewed on 2024-04-05 04:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:786 on 2024-04-05 04:21_

The removed logic is basically what `parse_name` does so let's delegate it.

---

_Marked ready for review by @dhruvmanila on 2024-04-05 04:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-05 04:23_

---

_@charliermarsh reviewed on 2024-04-05 04:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/parser/statement.rs`:2095 on 2024-04-05 04:25_

What does "the AST can't contain it" mean here?

---

_@charliermarsh approved on 2024-04-05 04:26_

---

_@dhruvmanila reviewed on 2024-04-05 04:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2095 on 2024-04-05 04:35_

There's no field to store an expression for `TypeVarTuple`:

https://github.com/astral-sh/ruff/blob/a516f8f1d86cfb49bc3a395cbe8a3af02857adc5/crates/ruff_python_ast/src/nodes.rs#L3156-L3161

while for `TypeVar` there is:

https://github.com/astral-sh/ruff/blob/a516f8f1d86cfb49bc3a395cbe8a3af02857adc5/crates/ruff_python_ast/src/nodes.rs#L3130-L3135

We'd need to parse the expression to highlight the correct range. Or, we could just check if there's a `:` token and provide the error message highlighting the `:` token.

---

_Comment by @github-actions[bot] on 2024-04-05 04:57_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:795 on 2024-04-05 06:34_

Are there other places where we parse a "type" expression? E.g. when parsing type arguments, type annotations etc. If so, I think it would make sense to have a `parse_type` or `parse_type_expression` method.

---

_@MichaReiser reviewed on 2024-04-05 06:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2095 on 2024-04-05 06:37_

I don't think I understand the comment. Does it mean we don't emit a syntax error or is it that Python emits a more specific syntax error? 


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2130 on 2024-04-05 06:38_

Are yield, etc expressions allowed in this position?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:805 on 2024-04-05 06:40_

Could we add some more invalid test cases:

```
type 
type x
type x = 
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2097 on 2024-04-05 06:41_

Same here. Could we add some more invalid and valid test cases (or did you already do so in another PR)?

For example, the example you mention above would be good (I suspect that fails with a syntax error today)

---

_@MichaReiser reviewed on 2024-04-05 06:41_

---

_@dhruvmanila reviewed on 2024-04-05 10:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2095 on 2024-04-05 10:15_

The latter. We do emit a syntax error. This comment is for a better error message if we want.

---

_@dhruvmanila reviewed on 2024-04-05 10:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:805 on 2024-04-05 10:20_

The problem with some of these cases (the top two in your list) is that the `type` is not a keyword but a name expression because of the soft keywords transformer. So, the parser never sees this as a type alias statement (at least for the first two statement).

That said, I think it makes sense to still add them either with a comment or as is so that in the future we can improve this.

---

_@dhruvmanila reviewed on 2024-04-05 10:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:795 on 2024-04-05 10:26_

Like for example a function parameter annotation? So, given

```py
def foo(x: int):
	pass

x: int = 1
type x = int
```

We parse all of the `int` expression using `parse_type_expression` method. Is this just to clarify that the parser is in a type context?

---

_@dhruvmanila reviewed on 2024-04-05 10:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2130 on 2024-04-05 10:27_

No, I've been holding off on adding those checks everywhere as I want to implement it once. But, it's a good idea to still add the test cases to verify the solution.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-05 10:52_

---

_Comment by @codspeed-hq[bot] on 2024-04-05 10:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/type-params)

### Merging #10785 will **not alter performance**

<sub>Comparing <code>dhruv/type-params</code> (cf2e597) with <code>dhruv/parser</code> (d8cd78c)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser reviewed on 2024-04-05 12:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:795 on 2024-04-05 12:30_

I think the main value is to have a place where we can reuse some of the expression validation logic and that I as a parse rule author don't always have to think about which expressions are valid (I can just call `parse_type_expression` and I know the parser will disallow expressions that aren't valid in a type context)

---

_@dhruvmanila reviewed on 2024-04-06 17:03_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:795 on 2024-04-06 17:03_

Right, that makes sense but I think the `parse_conditional_expression_or_higher` function should, in the future, do these validations.

---

_@dhruvmanila reviewed on 2024-04-09 08:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:795 on 2024-04-09 08:51_

> Right, that makes sense but I think the `parse_conditional_expression_or_higher` function should, in the future, do these validations.

Done in https://github.com/astral-sh/ruff/pull/10809

---

_Merged by @dhruvmanila on 2024-04-09 08:52_

---

_Closed by @dhruvmanila on 2024-04-09 08:52_

---

_Branch deleted on 2024-04-09 08:52_

---

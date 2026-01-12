```yaml
number: 10819
title: "Add tests for `async` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/async-stmt
created_at: 2024-04-07T15:49:33Z
updated_at: 2024-04-09T16:33:45Z
url: https://github.com/astral-sh/ruff/pull/10819
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for `async` statement

---

_@dhruvmanila_

## Summary

This PR adds test cases for the `async` statement.

---

_Label `parser` added by @dhruvmanila on 2024-04-07 15:49_

---

_Label `testing` added by @dhruvmanila on 2024-04-07 15:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 15:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2166 on 2024-04-08 08:58_

This recovery here slightly violates our recovery because we "drop" the async token. I wonder if we should instead parse this as a expression statement where `async` is an identifier.

```
async 
```

---

_@MichaReiser approved on 2024-04-08 08:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2166 on 2024-04-09 14:13_

Can you clarify which example are you referring to? Is it the one with double `async` keywords (`async async def foo(): ...`)?

---

_@dhruvmanila reviewed on 2024-04-09 14:13_

---

_Merged by @dhruvmanila on 2024-04-09 14:18_

---

_Closed by @dhruvmanila on 2024-04-09 14:18_

---

_Branch deleted on 2024-04-09 14:18_

---

_Comment by @codspeed-hq[bot] on 2024-04-09 14:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/async-stmt)

### Merging #10819 will **degrade performances by 8.21%**

<sub>Comparing <code>dhruv/async-stmt</code> (1d01988) with <code>dhruv/parser</code> (2e5128a)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/async-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/async-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.77% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.21% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.37% |


---

_Comment by @github-actions[bot] on 2024-04-09 14:32_

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

_@MichaReiser reviewed on 2024-04-09 16:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2166 on 2024-04-09 16:01_

It's probably seen in the existing tests as well. I think we shouldn't call `parse_statement` if we see a single `async` keyword. I think we should either:

* Do a lookahead and only parse `async` if we know we can parse out an async statement (a single `async` keyword than becomes an expression statement)
* Or, parse the `async` as an `ExpressionStatement` containing an `ASyncExpr` without any content.

The problem here is that we otherwise "drop" the `async` keyword. We bump the token when entering the function but then ignore it in the `parse_statement` call. You can see this for `async class X`. I think that should parse as a 

```
ExpressionStatement(AsyncExpression)
ClassDefinition
```

or 

```
ExpressionStatement(NameExpr(`async`))
ClassDefinition
```

but not as

```
ClassDefinition
```

because the `async` range then got completely omitted.

---

_@dhruvmanila reviewed on 2024-04-09 16:33_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2166 on 2024-04-09 16:33_

Oh ok, I see. Thanks for all the details!

---

```yaml
number: 10818
title: Add tests for decorators
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/decorators
created_at: 2024-04-07T15:48:53Z
updated_at: 2024-04-09T14:28:42Z
url: https://github.com/astral-sh/ruff/pull/10818
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for decorators

---

_@dhruvmanila_

## Summary

This PR adds test cases for the decorators.

---

_Label `parser` added by @dhruvmanila on 2024-04-07 15:48_

---

_Label `testing` added by @dhruvmanila on 2024-04-07 15:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 15:48_

---

_Comment by @codspeed-hq[bot] on 2024-04-07 15:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/decorators)

### Merging #10818 will **degrade performances by 8.21%**

<sub>Comparing <code>dhruv/decorators</code> (f4fd047) with <code>dhruv/parser</code> (9cbe614)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/decorators)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/decorators` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.72% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.21% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.38% |


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2229 on 2024-04-08 08:51_

Could we convert them into a binary expression where the left side is missing? TypeScript parses this as a `Decorator` and creates a `MissingDeclaration` node. https://astexplorer.net/#/gist/a5f4d9eabdade2111a1423af39012cb4/f58e76cd625d31670a149500f8cd3a23908a11dd

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_missing_expression.py.snap`:41 on 2024-04-08 08:53_

Huh, the AST here is a bit strange. Why is it not a decorator with a missing name?

---

_@MichaReiser approved on 2024-04-08 08:55_

---

_@dhruvmanila reviewed on 2024-04-09 12:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_missing_expression.py.snap`:41 on 2024-04-09 12:54_

The example in this case is:

```py
@def foo(): ...
```

Here, the parser sees `def` which doesn't match any expression start and fallback to `parse_name` as it's a keyword. Then it expects a NEWLINE as each decorator node ends with it but doesn't find it. The parser exists the decorator parsing loop.

Following token is not the start of function or class as only they can have the decorators so fallback to parsing `foo(): ...` as a statement. This looks like an annotated assignment statement so it parses it as such.

---

_@dhruvmanila reviewed on 2024-04-09 12:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_missing_expression.py.snap`:41 on 2024-04-09 12:58_

We could check if it's the start of an expression and only parse it if it is otherwise use an empty name node (not ideal, I know). Or rather I think I've a better solution.

---

_@dhruvmanila reviewed on 2024-04-09 12:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_missing_expression.py.snap`:41 on 2024-04-09 12:58_

(I'll put it up as a separate PR.)

---

_@dhruvmanila reviewed on 2024-04-09 14:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2229 on 2024-04-09 14:10_

Yeah, I've put it down as a possible solution. We would need to convert each decorator into a binary expression statement.

---

_Merged by @dhruvmanila on 2024-04-09 14:10_

---

_Closed by @dhruvmanila on 2024-04-09 14:10_

---

_Branch deleted on 2024-04-09 14:11_

---

_Comment by @github-actions[bot] on 2024-04-09 14:28_

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

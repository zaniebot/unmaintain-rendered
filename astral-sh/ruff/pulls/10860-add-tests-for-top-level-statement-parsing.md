```yaml
number: 10860
title: Add tests for top level statement parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-simple-stmts
created_at: 2024-04-10T14:13:33Z
updated_at: 2024-04-11T09:42:20Z
url: https://github.com/astral-sh/ruff/pull/10860
synced_at: 2026-01-10T22:37:01Z
```

# Add tests for top level statement parsing

---

_Pull request opened by @dhruvmanila on 2024-04-10 14:13_

## Summary

This PR adds test cases for top level statement parsing functions. This mainly tests the following:
1. Simple statements on same line with and without semicolons
2. Simple and compound statement on same line with and without semicolon
3. Compound statement and their respective clause on same line with and without semicolon (for example, `if True: pass else: pass`)

This PR also makes the following changes:
1. Use the current token change to highlight the above errors. Previously, it would use a combined range of previous statement _and_ current token.
2. Improved messages for the above errors.

For (1), you'll notice that some of the errors disappeared. The reason is that they now overlap with an existing error.


---

_Label `parser` added by @dhruvmanila on 2024-04-10 14:13_

---

_Label `testing` added by @dhruvmanila on 2024-04-10 14:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-10 14:13_

---

_@dhruvmanila reviewed on 2024-04-10 14:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:126 on 2024-04-10 14:16_

The reason this is required is because there's no dedicated node to capture a list of simple statements on the same line. This means that each statement needs to be parsed and returned one by one. This requires having a dedicated function to only parse a single statement.

Pyright has a dedicated node for this (`StatementListNode`) but this clutters the AST because then every single simple statement will be a `StatementListNode` with a single simple statement in it. [Pyright AST playground](https://zzzen.github.io/pyright-ast-viewer/#code/IYbgBARuDGBQywrOBLAZmAKgJwK4FMAuWMMUSEsyiIA)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:144 on 2024-04-10 14:18_

The change here is going from:
```
Syntax Error: use `;` to separate simple statements at byte range 0..11
  |
1 | x + y + z a + b + c
  | ^^^^^^^^^^^ Syntax Error: use `;` to separate simple statements
  |
```

To:
```
Errors:
-------
Syntax Error: Simple statements must be separated by newlines or semicolons at byte range 10..11
  |
1 | x + y + z a + b + c
  |           ^ Syntax Error: Simple statements must be separated by newlines or semicolons
  |
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:221 on 2024-04-10 14:19_

If there's no newline and it's not a compound statement, then we'll fallback to using `expect`. The bug which I was talking about on Discord.

---

_@dhruvmanila reviewed on 2024-04-10 14:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:144 on 2024-04-10 16:20_

Much better. It's now clear where I need to fix the error. Thanks for improving the message.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:148 on 2024-04-10 16:21_

Nit: You could move the `!heas_eaten_newline` one layer higher

```
if !has_eaten_newline {
    if !has_eaten_semicolon && self.at_simple_stmt() {
        ...
    } else if self.at_compount_stmt() {
        ...
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:195 on 2024-04-10 16:24_

Nit: I think that's covered by the same check on line 197 (but I'm okay keeping it)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:221 on 2024-04-10 16:26_

Nit: Could we push the error directly instead of calling `expect` or does that require a lot of code. The call looks redudant on the first glance (but it's not).

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@async_unexpected_token.py.snap`:224 on 2024-04-10 16:28_

Future: We should change the range to the start of the current token (an empty range), once our message rendering supports rendering empty ranges.

---

_@MichaReiser approved on 2024-04-10 16:31_

---

_@dhruvmanila reviewed on 2024-04-11 09:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:221 on 2024-04-11 09:17_

I see. Yeah, it does look redundant on first glance. I'll make the change.

---

_Comment by @codspeed-hq[bot] on 2024-04-11 09:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/test-simple-stmts)

### Merging #10860 will **degrade performances by 5.23%**

<sub>Comparing <code>dhruv/test-simple-stmts</code> (be7313d) with <code>dhruv/parser</code> (3603ca8)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/test-simple-stmts)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/test-simple-stmts` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.9 ms | 26.7 ms | +8.21% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.77% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.23% |


---

_Merged by @dhruvmanila on 2024-04-11 09:23_

---

_Closed by @dhruvmanila on 2024-04-11 09:23_

---

_Branch deleted on 2024-04-11 09:23_

---

_Comment by @github-actions[bot] on 2024-04-11 09:42_

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

```yaml
number: 10859
title: "Use `Semi` as terminator token for simple stmts"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/semicolon-terminator
created_at: 2024-04-10T14:09:26Z
updated_at: 2024-04-11T09:14:44Z
url: https://github.com/astral-sh/ruff/pull/10859
synced_at: 2026-01-12T15:55:33Z
```

# Use `Semi` as terminator token for simple stmts

---

_@dhruvmanila_

## Summary

This PR fixes a bug where the parser would error out when parsing simple statements separated by semicolon.

This only occurred in simple statement which uses list parsing. These are import statement, from import statement, global and nonlocal statement. The `Semi` token needs to be added as a list terminator in these cases.

### Other solution

A potential solution could be to use list parsing for a sequence of simple statements. I did think about this a bit but there I suspect there will be potential problems around semicolon, error recovery and newline token so I'm not doing it for now.

## Test Plan

Add test cases which verifies this bug.

---

_Label `bug` added by @dhruvmanila on 2024-04-10 14:09_

---

_Label `parser` added by @dhruvmanila on 2024-04-10 14:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-10 14:09_

---

_Comment by @codspeed-hq[bot] on 2024-04-10 14:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/semicolon-terminator)

### Merging #10859 will **degrade performances by 5.5%**

<sub>Comparing <code>dhruv/semicolon-terminator</code> (c3b9742) with <code>dhruv/parser</code> (f21b862)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/semicolon-terminator)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/semicolon-terminator` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.7 ms | 26.7 ms | +7.4% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 5.3 ms | 5 ms | +7.35% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.5% |


---

_Comment by @github-actions[bot] on 2024-04-10 14:29_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:910 on 2024-04-10 16:16_

Adding the `Semi` only to some of the recovery context feels a bit strange to me. What about if there's a semicolon in an array expression, shouldn't we not parse the semicolon there and consider what comes after to be a simple statement? 

It does feel kind of wrong but I wonder if `is_list_element` should return `true` when parsing a body. It feels wrong because `;` isn't actually a list element but it does mark the start of the next statement.

---

_@MichaReiser approved on 2024-04-10 16:17_

I'm feel conflicted about this change. I don't see a much better solution but only handling it in some recovery contexts feels incomplete (fixing one-offs instead of fixing the underlying problem).

I'm okay moving forward with this bu tI think we should follow up on the error recovery around semicolons.

---

_@dhruvmanila reviewed on 2024-04-11 09:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:910 on 2024-04-11 09:14_

I'm not a big fan of this either but the reason it exists only for a subset of recovery context is that the list of nodes being parsed is part of a simple statement _and_ is at the end of the statement. For example, `import <names>`.

I tried thinking of using list parsing for sequence of simple statements separated by semicolons (`parse_simple_statements`) but that also doesn't work. Should we use `;` as the terminator token? But that would still mean that the inner context got an unexpected token instead of a comma.

---

_Merged by @dhruvmanila on 2024-04-11 09:14_

---

_Closed by @dhruvmanila on 2024-04-11 09:14_

---

_Branch deleted on 2024-04-11 09:14_

---

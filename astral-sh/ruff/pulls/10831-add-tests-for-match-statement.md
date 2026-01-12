```yaml
number: 10831
title: "Add tests for `match` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/match-stmt
created_at: 2024-04-08T05:52:41Z
updated_at: 2024-04-09T16:32:32Z
url: https://github.com/astral-sh/ruff/pull/10831
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for `match` statement

---

_@dhruvmanila_

## Summary

This PR adds test cases for the `match` statement.

---

_Label `parser` added by @dhruvmanila on 2024-04-08 05:52_

---

_Label `testing` added by @dhruvmanila on 2024-04-08 05:52_

---

_Comment by @codspeed-hq[bot] on 2024-04-08 05:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-stmt)

### Merging #10831 will **degrade performances by 8.2%**

<sub>Comparing <code>dhruv/match-stmt</code> (7cfc054) with <code>dhruv/parser</code> (5cfeaa9)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/match-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.7 ms | -7.72% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.2% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.38% |


---

_Marked ready for review by @dhruvmanila on 2024-04-08 12:27_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-08 12:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2183 on 2024-04-09 08:40_

Should this be `expect` because the `Dedent` is required.

---

_@MichaReiser reviewed on 2024-04-09 08:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 08:42_

Should this use the list parsing infrastructure to ensure that the statement recovery correctly recovers when seeing a `case` keyword:


```
match x:
    case "test":
        async
                
    case "b": # The statement recovery should not eat over the `case` keyword
```

---

_@MichaReiser approved on 2024-04-09 08:44_

---

_@dhruvmanila reviewed on 2024-04-09 16:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 16:21_

I'm not sure which is why I didn't change. Can you give an example of where this would be useful? The provided one parses both `case` blocks as expected. We could probably use `parse_clauses` but that doesn't perform error recovery.

---

_Merged by @dhruvmanila on 2024-04-09 16:21_

---

_Closed by @dhruvmanila on 2024-04-09 16:21_

---

_Branch deleted on 2024-04-09 16:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 16:25_

I don't have a specific example but something where the expression parsing goes into recovery. It then skips over all tokens that aren't the start of a statement or expression, which is why I would expect that it skips over the `case` keyword. 

Or does that not happen today because the `case` keyword is parsed as a `Name` rather than the `case` keyword because the soft keyword tokenizer gets confused?

---

_@MichaReiser reviewed on 2024-04-09 16:25_

---

_@dhruvmanila reviewed on 2024-04-09 16:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 16:27_

I think it should be ok because of the `Dedent` token? I'll need to think about this but I do understand what you're trying to say.

---

_@MichaReiser reviewed on 2024-04-09 16:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 16:28_

> but I do understand what you're trying to say.

That's good haha

---

_@dhruvmanila reviewed on 2024-04-09 16:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 16:30_

Just to be clear, in case of any error in any of the case block, the parser should continue parsing the remaining case blocks as is, right? That is, it shouldn't consume the `case` keyword or name for the error recovery in previous case block.

---

_@MichaReiser reviewed on 2024-04-09 16:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2233 on 2024-04-09 16:32_

Yes, it shouldn't consume the `case` because it's a valid recovery point.

---

```yaml
number: 11871
title: Use re-lexing for normal list parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/re-lexing-2
created_at: 2024-06-14T12:41:20Z
updated_at: 2024-06-18T06:44:42Z
url: https://github.com/astral-sh/ruff/pull/11871
synced_at: 2026-01-10T21:56:00Z
```

# Use re-lexing for normal list parsing

---

_Pull request opened by @dhruvmanila on 2024-06-14 12:41_

## Summary

This PR is a follow-up on #11845 to add the re-lexing logic for normal list parsing.

A normal list parsing is basically parsing elements without any separator in between i.e., there can only be trivia tokens in between the two elements. Currently, this is only being used for parsing **assignment statement** and **f-string elements**. Assignment statements cannot be in a parenthesized context, but f-string can have curly braces so this PR is specifically for them.

I don't think this is an ideal recovery but the problem is that both lexer and parser could add an error for f-strings. If the lexer adds an error it'll emit an `Unknown` token instead while the parser adds the error directly. I think we'd need to move all f-string errors to be emitted by the parser instead. This way the parser can correctly inform the lexer that it's out of an f-string and then the lexer can pop the current f-string context out of the stack.

## Test Plan

Add test cases, update the snapshots, and run the fuzzer.

---

_Label `parser` added by @dhruvmanila on 2024-06-14 12:41_

---

_Comment by @codspeed-hq[bot] on 2024-06-14 12:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/re-lexing-2)

### Merging #11871 will **not alter performance**

<sub>Comparing <code>dhruv/re-lexing-2</code> (7543306) with <code>main</code> (8499abf)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-06-14 13:00_

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

_Marked ready for review by @dhruvmanila on 2024-06-17 10:03_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-17 10:03_

---

_@MichaReiser approved on 2024-06-18 06:33_

---

_Merged by @dhruvmanila on 2024-06-18 06:44_

---

_Closed by @dhruvmanila on 2024-06-18 06:44_

---

_Branch deleted on 2024-06-18 06:44_

---

```yaml
number: 11933
title: Avoid consuming trailing whitespace during re-lexing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/re-lex-comment-trailing-whitespace
created_at: 2024-06-19T02:52:28Z
updated_at: 2024-06-19T06:44:18Z
url: https://github.com/astral-sh/ruff/pull/11933
synced_at: 2026-01-12T15:55:39Z
```

# Avoid consuming trailing whitespace during re-lexing

---

_@dhruvmanila_

## Summary

This PR updates the re-lexing logic to avoid consuming the trailing whitespace and move the lexer explicitly to the last newline character encountered while moving backwards.

Consider the following code snippet as taken from the test case highlighted with whitespace (`.`) and newline (`\n`) characters:
```py
# There are trailing whitespace before the newline character but those whitespaces are
# part of the comment token
f"""hello {x # comment....\n
#                     ^
y = 1\n
```

The parser is at `y` when it's trying to recover from an unclosed `{`, so it calls into the re-lexing logic which tries to move the lexer back to the end of the previous line. But, as it consumed all whitespaces it moved the lexer to the location marked by `^` in the above code snippet. But, those whitespaces are part of the comment token. This means that the range for the two tokens were overlapping which introduced the panic.

Note that this is only a bug when there's a comment with a trailing whitespace otherwise it's fine to move the lexer to the whitespace character. This is because the lexer would just skip the whitespace otherwise. Nevertheless, this PR updates the logic to move it explicitly to the newline character in all cases.

fixes: #11929 

## Test Plan

Add test cases and update the snapshot. Make sure that it doesn't panic on the code snippet in the linked issue.


---

_Label `bug` added by @dhruvmanila on 2024-06-19 02:52_

---

_Label `parser` added by @dhruvmanila on 2024-06-19 02:52_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-19 02:52_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1391 on 2024-06-19 02:57_

Now, we encode both information in a single variable:
1. The lexer needs to be moved
2. The position the lexer needs to be moved to

---

_@dhruvmanila reviewed on 2024-06-19 02:57_

---

_Comment by @github-actions[bot] on 2024-06-19 03:12_

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

_@MichaReiser approved on 2024-06-19 05:20_

Makes sense

---

_Merged by @dhruvmanila on 2024-06-19 06:44_

---

_Closed by @dhruvmanila on 2024-06-19 06:44_

---

_Branch deleted on 2024-06-19 06:44_

---

```yaml
number: 12016
title: Use correct range to highlight line continuation error
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/unknown-range
created_at: 2024-06-25T03:14:19Z
updated_at: 2024-06-25T08:05:25Z
url: https://github.com/astral-sh/ruff/pull/12016
synced_at: 2026-01-10T21:56:00Z
```

# Use correct range to highlight line continuation error

---

_Pull request opened by @dhruvmanila on 2024-06-25 03:14_

## Summary

This PR fixes the range highlighted for the line continuation error.

Previously, it would highlight an incorrect range:
```
1 | call(a, b, \\\
  |           ^^ Syntax Error: unexpected character after line continuation character
2 | 
3 | def bar():
  |
```

And now:
```
  |
1 | call(a, b, \\\
  |             ^ Syntax Error: unexpected character after line continuation character
2 | 
3 | def bar():
  |
```

This is implemented by avoiding to update the token range for the `Unknown` token which is emitted when there's a lexical error. Instead, the `push_error` helper method will be responsible to update the range to the error location.

This actually becomes a requirement which can be seen in follow-up PRs.

## Test Plan

Update and validate the snapshot.


---

_Label `parser` added by @dhruvmanila on 2024-06-25 03:14_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-25 03:14_

---

_Comment by @github-actions[bot] on 2024-06-25 03:33_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:153 on 2024-06-25 06:21_

It's a bit unfortunate that we now have this branch in the very hot `next_token` function for the very rare case of an unknown token. But I don't see a better way of solving this that doesn't require a lot of repetition on the token range.

---

_@MichaReiser approved on 2024-06-25 06:22_

---

_@dhruvmanila reviewed on 2024-06-25 06:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:153 on 2024-06-25 06:26_

Yeah, I agree. I thought of doing something like what you've suggested (computing the token range at the place where it's emitted) but wanted to see if this actually impacts the performance. It doesn't seem to be which is why I'm fine moving ahead with this.

---

_Merged by @dhruvmanila on 2024-06-25 08:05_

---

_Closed by @dhruvmanila on 2024-06-25 08:05_

---

_Branch deleted on 2024-06-25 08:05_

---

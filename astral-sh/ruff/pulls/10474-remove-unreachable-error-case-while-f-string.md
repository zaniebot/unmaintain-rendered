```yaml
number: 10474
title: Remove unreachable error case while f-string parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/unreachable-f-string-parsing
created_at: 2024-03-19T14:17:17Z
updated_at: 2024-03-19T18:14:09Z
url: https://github.com/astral-sh/ruff/pull/10474
synced_at: 2026-01-12T15:55:32Z
```

# Remove unreachable error case while f-string parsing

---

_@dhruvmanila_

## Summary

This PR updates the f-string literal parsing code to remove the unreachable error handling and make it explicit that this is an unreachable branch.

As mentioned in https://github.com/astral-sh/ruff/pull/10406#discussion_r1530373513:

> Correct me if I'm wrong but I don't think this branch is even reachable because the `memchr3` call only tries to find either `{`, `}`, or `\` which the match statement covers up.
>
> Ok, so this code _is_ reachable but still the `ch` will always be `\` because the previous match case is something like:
>
> ```rs
> 	'\\' if !self.kind.is_raw_string() && self.peek_byte().is_some() => ...
> 	ch => ...
> ```
>
> I can confirm that by running the entire test suite.

## Test Plan

`cargo test`


---

_Label `parser` added by @dhruvmanila on 2024-03-19 14:17_

---

_Comment by @github-actions[bot] on 2024-03-19 14:39_

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

_Marked ready for review by @dhruvmanila on 2024-03-19 14:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-19 14:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:300 on 2024-03-19 16:27_

Nit: It took me a while to understand why only '{', '}', or '\\' are possible. It might be worth to add a comment or make the `unreachable!` more explicit to mention, Expected `{`, `}`, or `\\` but at `\\`

---

_@MichaReiser approved on 2024-03-19 16:27_

---

_Merged by @dhruvmanila on 2024-03-19 17:58_

---

_Closed by @dhruvmanila on 2024-03-19 17:58_

---

_Branch deleted on 2024-03-19 17:58_

---

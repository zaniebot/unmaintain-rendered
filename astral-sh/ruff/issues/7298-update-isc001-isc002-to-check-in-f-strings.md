```yaml
number: 7298
title: "Update `ISC001`, `ISC002` to check in f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
created_at: 2023-09-12T09:43:28Z
updated_at: 2023-09-20T11:58:23Z
url: https://github.com/astral-sh/ruff/issues/7298
synced_at: 2026-01-12T15:54:47Z
```

# Update `ISC001`, `ISC002` to check in f-strings

---

_@dhruvmanila_

`ISC001` and `ISC002` checks for implicitly concatenated strings over a single or multiple lines.

The rules needs to be updated to account for the f-strings. The implementation using ranges to check if 2 consecutive strings are implicitly concatenated.

### Proposed solution

1. We'll need to get the f-string range from the `Indexer` when we encounter a `FStringStart` token.
2. Another solution would be to consume all of the tokens from `FStringStart` to `FStringEnd` to get the range. I think using the `Indexer` is better.

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:48_

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:48_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-17 05:59_

---

_Closed by @dhruvmanila on 2023-09-20 11:58_

---

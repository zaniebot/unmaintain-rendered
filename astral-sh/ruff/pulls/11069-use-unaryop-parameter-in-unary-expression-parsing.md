```yaml
number: 11069
title: "Use `UnaryOp` parameter in unary expression parsing"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
draft: true
base: dhruv/refactor-compare-op
head: dhruv/refactor-unary-op
created_at: 2024-04-21T07:17:11Z
updated_at: 2024-04-21T15:55:24Z
url: https://github.com/astral-sh/ruff/pull/11069
synced_at: 2026-01-10T22:37:01Z
```

# Use `UnaryOp` parameter in unary expression parsing

---

_Pull request opened by @dhruvmanila on 2024-04-21 07:17_

## Summary

Part of #10752 

This PR updates unary expression parsing to take in the `UnaryOp` as a parameter.

## Test Plan

Make sure the unary expression parsing logic is still correct by running the test suite, fuzz testing it and running it against a dozen or so open-source repositories.


---

_Label `internal` added by @dhruvmanila on 2024-04-21 07:17_

---

_Label `parser` added by @dhruvmanila on 2024-04-21 07:17_

---

_Comment by @dhruvmanila on 2024-04-21 15:55_

Closed in favor of #11073 

---

_Closed by @dhruvmanila on 2024-04-21 15:55_

---

```yaml
number: 11459
title: Bump soft keyword as name token
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/bump-token
created_at: 2024-05-17T11:47:40Z
updated_at: 2024-05-17T11:47:54Z
url: https://github.com/astral-sh/ruff/pull/11459
synced_at: 2026-01-12T15:55:38Z
```

# Bump soft keyword as name token

---

_@dhruvmanila_

## Summary

This PR updates various `bump` methods to allow parser to bump the soft keyword as a name token.

This is required because the token vector should store the resolved token. Otherwise, the token vector and the AST are out of sync.

The process is as follows:
* One common method to bump the given kind for the parser which is `do_bump`
* This calls in the `bump` method on the token source
* The token source adds the given token kind and bump itself to the next non-trivia token
* While doing this bump, it still adds the trivia tokens to the token vector

The end result is that the parser informs the token source to add the given kind to the token vector and move on to the next token.

Here, we can then introduce a `bump_soft_keyword_as_name` method which asserts that the current token is a soft keyword and bumps it as a name token instead. The `parse_identifier` method then calls the new method instead.


---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-17 11:47_

---

_Merged by @dhruvmanila on 2024-05-17 11:47_

---

_Closed by @dhruvmanila on 2024-05-17 11:47_

---

_Branch deleted on 2024-05-17 11:47_

---

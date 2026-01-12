```yaml
number: 11442
title: "Use lookahead to resolve `type` soft keyword"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/soft-keyword-type
created_at: 2024-05-16T09:33:33Z
updated_at: 2024-05-21T04:44:52Z
url: https://github.com/astral-sh/ruff/pull/11442
synced_at: 2026-01-12T15:55:38Z
```

# Use lookahead to resolve `type` soft keyword

---

_@dhruvmanila_

## Summary

This PR updates the `type` alias statement parsing to use lookahead to resolve whether it's used as a keyword or as an identifier depending on the context.

The strategy here is that the first token should be either a name or a soft keyword and the second can be either a `[` or `=`. Remember that `type type = int` is valid where the first `type` is a keyword while the second `type` is an identifier.


---

_Marked ready for review by @dhruvmanila on 2024-05-20 06:41_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-20 06:41_

---

_@charliermarsh approved on 2024-05-20 15:42_

---

_Label `parser` added by @dhruvmanila on 2024-05-21 04:44_

---

_Merged by @dhruvmanila on 2024-05-21 04:44_

---

_Closed by @dhruvmanila on 2024-05-21 04:44_

---

_Branch deleted on 2024-05-21 04:44_

---

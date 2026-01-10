```yaml
number: 7043
title: Add support for PEP 701 in the parser
type: issue
state: closed
author: dhruvmanila
labels:
  - parser
  - python312
assignees: []
created_at: 2023-09-01T13:37:40Z
updated_at: 2023-09-15T07:31:35Z
url: https://github.com/astral-sh/ruff/issues/7043
synced_at: 2026-01-10T11:09:49Z
```

# Add support for PEP 701 in the parser

---

_Issue opened by @dhruvmanila on 2023-09-01 13:37_

The task is to use the new tokens emitted by the lexer (#7042) and create the `ExprFString` node using it. This will require major refactor in `string.rs` to support this.

### Relevant PRs:

- https://github.com/astral-sh/ruff/pull/7041
- https://github.com/astral-sh/ruff/pull/7211
- https://github.com/astral-sh/ruff/pull/7263
- #7331
- #7378 

---

_Label `parser` added by @dhruvmanila on 2023-09-01 13:37_

---

_Label `python312` added by @dhruvmanila on 2023-09-01 13:37_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-01 13:37_

---

_Closed by @dhruvmanila on 2023-09-15 07:31_

---

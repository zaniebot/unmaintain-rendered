```yaml
number: 11072
title: "Parser drops the `not` token during comparison expression parsing"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
created_at: 2024-04-21T14:51:36Z
updated_at: 2024-04-23T04:42:42Z
url: https://github.com/astral-sh/ruff/issues/11072
synced_at: 2026-01-12T15:54:50Z
```

# Parser drops the `not` token during comparison expression parsing

---

_@dhruvmanila_

For example:
```python
not a in b not c
```

Here, the second `not` token is being dropped during the comparison expression parsing.

The reason is that the current token in the loop is being bumped without checking if the next token is `in` (making it `not in`).

---

_Label `bug` added by @dhruvmanila on 2024-04-21 14:51_

---

_Label `parser` added by @dhruvmanila on 2024-04-21 14:51_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-21 14:51_

---

_Closed by @dhruvmanila on 2024-04-23 04:42_

---

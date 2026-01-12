```yaml
number: 11639
title: Update with-items parsing to use speculative parsing
type: issue
state: closed
author: dhruvmanila
labels:
  - parser
assignees: []
created_at: 2024-05-31T14:23:31Z
updated_at: 2024-06-06T08:59:57Z
url: https://github.com/astral-sh/ruff/issues/11639
synced_at: 2026-01-12T15:54:51Z
```

# Update with-items parsing to use speculative parsing

---

_@dhruvmanila_

https://github.com/astral-sh/ruff/pull/11457 added the ability to do speculative parsing via checkpoint - rewind infrastructure and is currently being used for `match` statement via https://github.com/astral-sh/ruff/pull/11443.

To give a brief background, **speculative parsing** means that the parser tries to parse a syntax node as one kind and determines at the end if the assumption was right by testing if the parser is at a specific token (or has no errors). This is useful if a syntax is ambiguous and no amount of lookahead (except parsing the whole syntax) is sufficient to determine what syntax it is.

The idea here is to update the current parsing logic for with-items to use the checkpoint - rewind infrastructure. The current logic performs a manual and a bit complex speculative parsing to disambiguate between a parenthesized with-items and parenthesized with-item expression i.e., whether the parenthesis belongs to with-items or the expression of the first with-item. For example,

```py
with (item1, item2): ...       # Parenthesized with-items
with (item1, item2) as f: ...  # Parenthesized expression
```


---

_Label `parser` added by @dhruvmanila on 2024-05-31 14:23_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-05-31 14:23_

---

_Renamed from "Update with-items parsing to use checkpoint - rewind infrastructure" to "Update with-items parsing to use speculative parsing" by @dhruvmanila on 2024-06-04 05:19_

---

_Closed by @dhruvmanila on 2024-06-06 08:59_

---

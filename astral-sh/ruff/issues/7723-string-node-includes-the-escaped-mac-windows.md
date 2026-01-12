```yaml
number: 7723
title: String node includes the escaped mac / windows newline character
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
created_at: 2023-09-30T19:01:57Z
updated_at: 2023-10-01T02:08:00Z
url: https://github.com/astral-sh/ruff/issues/7723
synced_at: 2026-01-12T15:54:47Z
```

# String node includes the escaped mac / windows newline character

---

_@dhruvmanila_

This is not easily demonstrable in the playground.

Note: Replace the newline characters with the actual newline when testing.

The issue here I see is that for a newline character (`\n`) such as in the following code:
```python
"text \\n
more text"
```

The AST node will be:
```rust
StringConstant {
	value: "text more text",
	unicode: false,
	implicit_concatenated: false,
}
```

Notice that the escaped newline are not part of the actual string value which is correct. But if the mac (`\r`) or windows (`\r\n`) newline character is used then they get included in the string constant:

```rust
StringConstant {
	value: "text \\\r\nmore text",
	unicode: false,
	implicit_concatenated: false,
}
// OR
StringConstant {
	value: "text \\\rmore text",
	unicode: false,
	implicit_concatenated: false,
}
```

Discovered in #7722 

---

_Label `bug` added by @dhruvmanila on 2023-09-30 19:01_

---

_Label `parser` added by @dhruvmanila on 2023-09-30 19:01_

---

_Closed by @dhruvmanila on 2023-10-01 02:08_

---

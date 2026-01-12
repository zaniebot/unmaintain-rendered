```yaml
number: 9303
title: "ISC Rule suggestion: Consistent no leading whitespace for multiline concatenation"
type: issue
state: open
author: mroeschke
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-12-28T20:24:45Z
updated_at: 2023-12-31T12:38:56Z
url: https://github.com/astral-sh/ruff/issues/9303
synced_at: 2026-01-12T15:54:49Z
```

# ISC Rule suggestion: Consistent no leading whitespace for multiline concatenation

---

_@mroeschke_

As of ruff `0.1.6`, `ISC002` accepts both the following multi-line string concatenation

```python
z = (
    "The quick brown fox jumps over the lazy "
    "dog."
)
a = (
    "The quick brown fox jumps over the lazy"
    " dog."
)
```

It would be nice to have an additional rule where for multi-line concatenation that the next line does not start with a white space for visual consistency (e.g. enforce the format of `z`). I _think_ the previous line could always have a optional trailing white space.

---

_Label `rule` added by @charliermarsh on 2023-12-31 12:38_

---

_Label `needs-decision` added by @charliermarsh on 2023-12-31 12:38_

---

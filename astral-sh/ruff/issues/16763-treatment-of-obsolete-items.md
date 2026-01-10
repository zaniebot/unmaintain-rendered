```yaml
number: 16763
title: Treatment of obsolete items
type: issue
state: closed
author: ya7010
labels:
  - question
  - needs-info
assignees: []
created_at: 2025-03-15T10:23:49Z
updated_at: 2025-03-16T09:10:40Z
url: https://github.com/astral-sh/ruff/issues/16763
synced_at: 2026-01-10T11:09:57Z
```

# Treatment of obsolete items

---

_Issue opened by @ya7010 on 2025-03-15 10:23_

### Question

I noticed that ruff's rules are not referenced correctly in pyproject.toml and have updated the JSON Schema Store. https://github.com/SchemaStore/schemastore/pull/4569

There are already obsolete items and rules, is it possible to keep them in the JSON Schema?

I have marked them obsolete manually, but it should be possible to automatically generate obsolete settings in JSON Schema.

### Version

_No response_

---

_Label `question` added by @ya7010 on 2025-03-15 10:23_

---

_Comment by @ya7010 on 2025-03-15 10:27_

Reference: https://github.com/SchemaStore/schemastore/blob/19aab1842feb4cd54b603a4b4c8f7345693ce746/src/schemas/json/ruff.json#L3691



---

_Comment by @MichaReiser on 2025-03-15 15:10_

I'm not sure I understand your issue. Can you give me a concrete example of something that isn't working with today's schema?

---

_Label `needs-info` added by @MichaReiser on 2025-03-15 15:10_

---

_Comment by @ya7010 on 2025-03-15 15:29_

This is what I encountered.

https://github.com/python-pendulum/pendulum/blob/76422468575b5817f7a58f3d05c26b480bf671a9/pyproject.toml#L86

"TCH" is currently unavailable.
However, older projects still specify it, so using JSON Schema will result in an error.

---

_Comment by @MichaReiser on 2025-03-16 09:10_

Yeah, that makes sense. We're aware of this issue and it isn't just specific to rules (although that's where it's most apparent). 

---

_Closed by @MichaReiser on 2025-03-16 09:10_

---

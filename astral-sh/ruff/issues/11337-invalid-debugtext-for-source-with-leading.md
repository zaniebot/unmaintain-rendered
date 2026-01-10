```yaml
number: 11337
title: "Invalid `DebugText` for source with leading whitespace"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
created_at: 2024-05-08T13:11:56Z
updated_at: 2024-06-11T02:14:05Z
url: https://github.com/astral-sh/ruff/issues/11337
synced_at: 2026-01-10T11:09:53Z
```

# Invalid `DebugText` for source with leading whitespace

---

_Issue opened by @dhruvmanila on 2024-05-08 13:11_

Given:

```py
        
# ^^^^^^ trailing whitespace here
f"{x   =  }"
```

The parser produces the incorrect AST. This is because the [`Parser::src_text`](https://github.com/astral-sh/ruff/blob/22639c5a2aabb21b97b1f3b6366e1fe5d300d572/crates/ruff_python_parser/src/parser/mod.rs#L467-L476) creates an offset which doesn't take whitespace into consideration.

https://play.ruff.rs/dbc4339d-c692-403e-9fbc-6a33ca1d299e

---

_Label `bug` added by @dhruvmanila on 2024-05-08 13:11_

---

_Label `parser` added by @dhruvmanila on 2024-05-08 13:11_

---

_Comment by @dhruvmanila on 2024-05-08 13:14_

This basically affects all nodes that uses `src_text` method.

---

_Comment by @dhruvmanila on 2024-06-11 02:14_

resolved in #11457 

---

_Closed by @dhruvmanila on 2024-06-11 02:14_

---

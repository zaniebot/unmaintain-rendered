```yaml
number: 1007
title: Render google-style docstrings to markdown
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-08-15T14:21:53Z
updated_at: 2025-11-21T16:01:37Z
url: https://github.com/astral-sh/ty/issues/1007
synced_at: 2026-01-10T01:58:59Z
```

# Render google-style docstrings to markdown

---

_Issue opened by @Gankra on 2025-08-15 14:21_

Currently we punt on this and just wrap the raw contents in a plaintext codeblock: 
https://github.com/astral-sh/ruff/blob/6de84ed56eac4f1f3efe9d8a9d47c9b30a445707/crates/ty_ide/src/docstring.rs#L65-L73

The format looks like this:

```
This is a function description.

Args:
    param1 (str): The first parameter description
    param2 (int): The second parameter description
        This is a continuation of param2 description.
    param3: A parameter without type annotation

Returns:
    str: The return value description
```

This is an interesting case where doing a *partial* job can often be worse than doing nothing at all, as the current output is *readable* but ugly. A partial job may end up being unreadable. I've also received warning that these formats tend to be un(der)specified, and even if they weren't, you need to handle malformed inputs gracefully.


---

_Added to milestone `GA` by @Gankra on 2025-08-15 14:21_

---

_Label `server` added by @Gankra on 2025-08-15 14:21_

---

_Assigned to @Gankra by @Gankra on 2025-11-14 13:57_

---

_Comment by @Gankra on 2025-11-21 16:01_

MVP implemented by https://github.com/astral-sh/ruff/pull/21550

---

_Closed by @Gankra on 2025-11-21 16:01_

---

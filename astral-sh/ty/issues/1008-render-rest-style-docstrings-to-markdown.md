```yaml
number: 1008
title: Render reST-style docstrings to markdown
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-08-15T14:24:46Z
updated_at: 2025-11-21T16:01:32Z
url: https://github.com/astral-sh/ty/issues/1008
synced_at: 2026-01-12T15:54:24Z
```

# Render reST-style docstrings to markdown

---

_@Gankra_

Currently we punt on this and just wrap the raw contents in a plaintext codeblock: 
https://github.com/astral-sh/ruff/blob/6de84ed56eac4f1f3efe9d8a9d47c9b30a445707/crates/ty_ide/src/docstring.rs#L65-L73

The format looks like this:

```
This is a function description.

:param str param1: The first parameter description
:param int param2: The second parameter description
    This is a continuation of param2 description.
:param param3: A parameter without type annotation
:returns: The return value description
:rtype: str
```

This is an interesting case where doing a *partial* job can often be worse than doing nothing at all, as the current output is *readable* but ugly. A partial job may end up being unreadable. I've also received warning that these formats tend to be un(der)specified, and even if they weren't, you need to handle malformed inputs gracefully.


---

_Added to milestone `GA` by @Gankra on 2025-08-15 14:24_

---

_Label `server` added by @Gankra on 2025-08-15 14:24_

---

_Assigned to @Gankra by @Gankra on 2025-11-14 13:57_

---

_Comment by @Gankra on 2025-11-21 16:01_

MVP implemented by https://github.com/astral-sh/ruff/pull/21550

---

_Closed by @Gankra on 2025-11-21 16:01_

---

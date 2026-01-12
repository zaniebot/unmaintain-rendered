```yaml
number: 898
title: Include class docstring in signature help for constructor calls
type: issue
state: closed
author: UnboundVariable
labels:
  - server
assignees: []
created_at: 2025-07-26T18:09:43Z
updated_at: 2025-09-03T02:34:03Z
url: https://github.com/astral-sh/ty/issues/898
synced_at: 2026-01-12T15:54:24Z
```

# Include class docstring in signature help for constructor calls

---

_@UnboundVariable_

The "signature help" language server feature currently includes the docstring for the target function or method, but it doesn't have any special logic for constructor calls. It may be desirable to include the docstring for the class in addition to (or in place of) the docstring for the `__new__` or `__init__` method. I've left a TODO in the `signature_help.rs` module to revisit this based on user feedback.

Feedback from ty users would be helpful here. What would you like to see displayed in this case?

---

_Label `server` added by @MichaReiser on 2025-07-26 18:33_

---

_Comment by @MatthewMckee4 on 2025-07-26 21:51_

Would definitely be interesting to hear others thoughts.

I think it's most useful to show the actual function docstring that is being called (meta class `__call__`, `__new__` or `__init__`)

To me the class docstring isn't relevant here and people often use it to show other information (attributes, methods)


---

_Comment by @AKuederle on 2025-08-05 16:27_

Really depends on the internal coding conventions. I work a lot with "dataclass" like classes. As the `__init__` does nothing besides setting parameters, we usually don't add a docstring for it and have all documentation within the class docstring.

The only save way would be to provide both, or maybe to have a reasonable fallback to the class docstring, if the `__init__` does not provide one.

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 07:03_

---

_Assigned to @Gankra by @Gankra on 2025-08-19 12:25_

---

_Comment by @Gankra on 2025-08-20 21:15_

> The only save way would be to provide both, or maybe to have a reasonable fallback to the class docstring, if the __init__ does not provide one.

This is exactly the rule that I intuitively thought of too, so it's what I'm working on implementing (and if it turns out we want to render both then that's a small tweak to the implementation).

---

_Closed by @Gankra on 2025-09-02 18:49_

---

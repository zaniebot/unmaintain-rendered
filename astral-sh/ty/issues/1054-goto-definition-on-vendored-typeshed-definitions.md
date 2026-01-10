```yaml
number: 1054
title: "goto-definition on vendored typeshed definitions doesn't work"
type: issue
state: closed
author: Gankra
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-19T17:19:31Z
updated_at: 2025-10-21T17:38:42Z
url: https://github.com/astral-sh/ty/issues/1054
synced_at: 2026-01-10T02:06:24Z
```

# goto-definition on vendored typeshed definitions doesn't work

---

_Issue opened by @Gankra on 2025-08-19 17:19_

This is a followup to #1004. 

It's exactly the same issue but specifically for when goto-declaration opens up a copy of our vendored typeshed in your IDE.

```python
from warnings import deprecated
```

If you goto-declaration on "deprecated" it will open up our vendored typeshed in the IDE.
If you goto-definition on "deprecated" it will open up the actual stdlib in the IDE.
...but if you goto-definition on *the vendored typeshed definition* it won't go anywhere.

From initial debugging it seems like file_to_module doesn't recognize the opened file, and so stub mapping fails. I assume there's some trickery going on with vendored typeshed files that makes this file a different one from the one used normally in our stdlib analysis.

---

_Label `bug` added by @Gankra on 2025-08-19 17:19_

---

_Label `server` added by @Gankra on 2025-08-19 17:19_

---

_Added to milestone `Beta` by @Gankra on 2025-08-19 17:22_

---

_Comment by @MichaReiser on 2025-08-19 17:27_

> From initial debugging it seems like file_to_module doesn't recognize the opened file, and so stub mapping fails. I assume there's some trickery going on with vendored typeshed files that makes this file a different one from the one used normally in our stdlib analysis.

That makes sense to me. Does it, by chance work if we don't fallback to use the default database here and instead use the top-level project as fallback here

https://github.com/astral-sh/ruff/blob/fc72ff4a946e4f044911ada12986a9f8731c812c/crates/ty_server/src/session.rs#L338-L343

You might also need to change this in other places where we fall back to the default db

My thinking here is that `file_to_module` might succeed if we use the correct set of search paths.

---

_Assigned to @Gankra by @Gankra on 2025-08-19 17:29_

---

_Comment by @MichaReiser on 2025-08-19 17:29_

The other possibility is that we need some form of reverse mapping for

https://github.com/astral-sh/ruff/blob/53d795da6702427a64d45044290a9d226c4e97e4/crates/ty_server/src/system.rs#L28-L43

Thinking about it more. I think my suggestion above is rubbish :) 

---

_Comment by @Gankra on 2025-08-19 17:37_

Ah yeah that's exactly the kind of logic I was imagining might throw a spanner in the works.

---

_Closed by @Gankra on 2025-10-21 17:38_

---

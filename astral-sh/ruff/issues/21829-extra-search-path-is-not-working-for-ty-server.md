```yaml
number: 21829
title: "--extra-search-path is not working for ty server?"
type: issue
state: closed
author: danielpopescu
labels:
  - question
assignees: []
created_at: 2025-12-06T22:35:25Z
updated_at: 2025-12-07T14:20:04Z
url: https://github.com/astral-sh/ruff/issues/21829
synced_at: 2026-01-10T11:10:00Z
```

# --extra-search-path is not working for ty server?

---

_Issue opened by @danielpopescu on 2025-12-06 22:35_

### Question

How does one add additional stub files to the server? The checker supports --extra-search-path but the server not... 

Main use case here is for adding important stub files like matplotlib or sklearn (which provide stubs outside the typeshed)..

Thank you

### Version

_No response_

---

_Label `question` added by @danielpopescu on 2025-12-06 22:35_

---

_Comment by @MichaReiser on 2025-12-07 10:39_

For now, you have to create a create a configuration file (ty.toml or pyproject.toml). We plan to support in editor configuration in the future.

---

_Comment by @danielpopescu on 2025-12-07 14:19_

Got it. Ty.

---

_Closed by @danielpopescu on 2025-12-07 14:19_

---

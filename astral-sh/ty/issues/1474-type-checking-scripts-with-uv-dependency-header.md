```yaml
number: 1474
title: Type checking scripts with uv dependency header
type: issue
state: closed
author: PurpleMyst
labels:
  - question
assignees: []
created_at: 2025-11-04T00:07:07Z
updated_at: 2025-11-04T04:30:46Z
url: https://github.com/astral-sh/ty/issues/1474
synced_at: 2026-01-10T02:06:25Z
```

# Type checking scripts with uv dependency header

---

_Issue opened by @PurpleMyst on 2025-11-04 00:07_

### Question

As per the title, how is it possible to typecheck a script whose dependencies are defined in a script manifest header, such as:
```python
# /// script
# requires-python = ">=3.11"
# dependencies = [
#     "typer",
#     "python-dotenv",
#     "requests",
#     "termcolor",
#     "tomlkit",
#     "tabulate",
# ]
# ///
# [script omitted]
```

### Version

ty 0.0.1-alpha.25 (3abd4c968 2025-10-29)

---

_Label `question` added by @PurpleMyst on 2025-11-04 00:07_

---

_Closed by @AlexWaygood on 2025-11-04 04:30_

---

_Comment by @AlexWaygood on 2025-11-04 04:30_

Thanks for opening the issue! Please see #691

---

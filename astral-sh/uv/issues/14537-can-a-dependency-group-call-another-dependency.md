```yaml
number: 14537
title: "Can a dependency group \"call\" another dependency group?"
type: issue
state: closed
author: stdedos
labels:
  - question
assignees: []
created_at: 2025-07-10T11:17:29Z
updated_at: 2025-07-10T11:28:42Z
url: https://github.com/astral-sh/uv/issues/14537
synced_at: 2026-01-12T16:01:50Z
```

# Can a dependency group "call" another dependency group?

---

_@stdedos_

### Question

```toml
[dependency-groups]
dev = [
    "ipython>=9.4.0",
    "mypy~=1.16",
    "pudb>=2025.1",
    "pylint~=3.0",
    "setuptools-scm>=8.3.1",
    "typing-stuff",
]
typing-stuff = [
    "types-paramiko",
]
```

I have read https://docs.astral.sh/uv/concepts/projects/dependencies/, but I don't seem to get hints either direction

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @stdedos on 2025-07-10 11:17_

---

_Comment by @zanieb on 2025-07-10 11:24_

Here you go https://peps.python.org/pep-0735/#dependency-group-include

---

_Assigned to @zanieb by @zanieb on 2025-07-10 11:24_

---

_Closed by @stdedos on 2025-07-10 11:28_

---

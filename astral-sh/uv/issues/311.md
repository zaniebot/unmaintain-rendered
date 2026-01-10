```yaml
number: 311
title: Track and report the sources of requirements
type: issue
state: open
author: zanieb
labels:
  - error messages
  - tracing
assignees: []
created_at: 2023-11-03T15:39:52Z
updated_at: 2024-02-29T16:22:40Z
url: https://github.com/astral-sh/uv/issues/311
synced_at: 2026-01-10T05:40:31Z
```

# Track and report the sources of requirements

---

_Issue opened by @zanieb on 2023-11-03 15:39_

i.e. if I provide multiple requirement files I want to know which one is responsible for each incompatible requirement

---

_Comment by @zanieb on 2023-11-03 16:50_

This is low-priority as it is only useful for `pip-compile` and not necessary sensible in the `pyproject.toml` centered use-case we care about long-term.

---

_Comment by @Oblynx on 2024-02-29 13:32_

It would be useful in our use case where we compile together the requirements of multiple packages, each with its own pyproject.toml and requirements.in

---

_Label `error messages` added by @zanieb on 2024-02-29 16:22_

---

_Label `tracing` added by @zanieb on 2024-02-29 16:22_

---

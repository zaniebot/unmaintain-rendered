---
number: 15698
title: When running the uv run command, it updates the Python environment by default. How can I configure it to only use the current environment without checking for consistency with the pyproject.toml file?
type: issue
state: closed
author: shell-nlp
labels:
  - question
assignees: []
created_at: 2025-09-05T09:52:41Z
updated_at: 2025-09-05T10:22:24Z
url: https://github.com/astral-sh/uv/issues/15698
synced_at: 2026-01-10T01:25:58Z
---

# When running the uv run command, it updates the Python environment by default. How can I configure it to only use the current environment without checking for consistency with the pyproject.toml file?

---

_Issue opened by @shell-nlp on 2025-09-05 09:52_

### Question

When running the uv run command, it updates the Python environment by default. How can I configure it to only use the current environment without checking for consistency with the pyproject.toml file?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @shell-nlp on 2025-09-05 09:52_

---

_Comment by @konstin on 2025-09-05 09:54_

Do you mean `--no-sync`? Why do want an environment that is inconsistent with the declared dependencies?

---

_Comment by @shell-nlp on 2025-09-05 10:03_

I needed this for a specific reason—really appreciate your help! 

---

_Closed by @shell-nlp on 2025-09-05 10:22_

---

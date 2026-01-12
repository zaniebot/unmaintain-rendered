```yaml
number: 12109
title: "Cross-system migration [ubuntu-->centos7], can't activate the virtual environment problem"
type: issue
state: open
author: software-defetc-prediction-research
labels:
  - question
assignees: []
created_at: 2025-03-11T09:26:50Z
updated_at: 2025-03-11T09:40:34Z
url: https://github.com/astral-sh/uv/issues/12109
synced_at: 2026-01-12T16:00:55Z
```

# Cross-system migration [ubuntu-->centos7], can't activate the virtual environment problem

---

_@software-defetc-prediction-research_

### Question

Question 1: The original environment is in Ubuntu, using uv to create a virtual environment, after installing the relevant dependencies, it runs normally, now the code is migrated to centos 7, and it can't activate the virtual environment under .venv, how to solve it?
Question 2: In an intranet environment, I can't install uv over the internet, but I download the whl file offline to install it, but it says that the whl file doesn't support the platform or it's not valid.

### Platform

centos 7 x86_84

### Version

centos linux 7.9.2009

---

_Label `question` added by @software-defetc-prediction-research on 2025-03-11 09:26_

---

_Comment by @konstin on 2025-03-11 09:40_

For question 1, can you `uv sync` again? Virtual environments aren't portable between systems or paths, they need to recreated.

For question 2, can you share instructions how to reproduce the problem, the list of wheel (`ls -l`) and the verbose log of the error (running uv with `-v`)?

---

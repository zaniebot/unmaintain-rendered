```yaml
number: 12554
title: UV Sync Globally with Flag like Pip
type: issue
state: closed
author: Lyranile
labels:
  - question
assignees: []
created_at: 2025-03-30T08:04:34Z
updated_at: 2025-03-30T15:04:03Z
url: https://github.com/astral-sh/uv/issues/12554
synced_at: 2026-01-12T16:01:06Z
```

# UV Sync Globally with Flag like Pip

---

_@Lyranile_

### Summary

# Feature
I need uv sync to install python packages in the global directory, the same as pip
It was possible with poetry using the virtualenv create to false, I want the same in uv

## Background
I'm using an internal docker image with some packages that are installed globally with pip
I'm using UV with pyproject in my code and I want the packages to work with, say, pytest that is installed globally


### Example

uv sync --system would be great
It is also like uv pip install --system that is already implemented

---

_Label `enhancement` added by @Lyranile on 2025-03-30 08:04_

---

_Comment by @charliermarsh on 2025-03-30 15:03_

You can set `UV_PROJECT_ENVIRONMENT` to the system Python. It's documented here: https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path.

---

_Closed by @charliermarsh on 2025-03-30 15:04_

---

_Label `enhancement` removed by @charliermarsh on 2025-03-30 15:04_

---

_Label `question` added by @charliermarsh on 2025-03-30 15:04_

---

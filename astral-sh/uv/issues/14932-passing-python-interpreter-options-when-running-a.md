---
number: 14932
title: Passing Python interpreter options when running a uv-managed Python script
type: issue
state: open
author: dclong
labels:
  - question
assignees: []
created_at: 2025-07-28T05:36:29Z
updated_at: 2025-07-28T13:16:12Z
url: https://github.com/astral-sh/uv/issues/14932
synced_at: 2026-01-10T01:25:50Z
---

# Passing Python interpreter options when running a uv-managed Python script

---

_Issue opened by @dclong on 2025-07-28 05:36_

### Question

Is it possible to pass Python interpreter options (e.g., `python -P`) when running a uv-managed Python script? If I use something like `uv run python -P /path/to/uv_init_script`, it won't automatically resolve and install dependencies specified in the script. 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @dclong on 2025-07-28 05:36_

---

_Comment by @zanieb on 2025-07-28 13:15_

I'd use environment variables, e.g., `PYTHONSAFEPATH=1`

---

_Comment by @zanieb on 2025-07-28 13:16_

Otherwise, this needs https://github.com/astral-sh/uv/pull/12763

---

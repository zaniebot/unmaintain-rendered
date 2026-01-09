---
number: 14472
title: How to specify a shebang to run a streamlit app python file with inline requirements?
type: issue
state: open
author: AY1uZwIcJzKhaGywovQP
labels:
  - question
assignees: []
created_at: 2025-07-07T02:22:54Z
updated_at: 2025-07-23T01:30:26Z
url: https://github.com/astral-sh/uv/issues/14472
synced_at: 2026-01-07T13:12:18-06:00
---

# How to specify a shebang to run a streamlit app python file with inline requirements?

---

_Issue opened by @AY1uZwIcJzKhaGywovQP on 2025-07-07 02:22_

### Question

Hi,

A common usage of mine is to use uv to run scripts with inline requirements in the form:
```python
#!/usr/bin/env -S uv
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "pandas>=2.2.0,<3.0.0",
# ]
# ///
...
```
However, we run streamlit apps using the command `streamlit run`. How would I modify the shebang such that uv would still use the specified inline requirements and then run that file with the command `streamlit run`?

### Platform

Linux 6.8.0-60-generic x86_64 GNU/Linux

### Version

uv 0.7.19

---

_Label `question` added by @AY1uZwIcJzKhaGywovQP on 2025-07-07 02:22_

---

_Comment by @j-muller on 2025-07-22 16:21_

Same question here and did not find a solution yet.

Also, it is not possible to run the Streamlit server from the code as there's no proper public API to do so. Some people tried to do it in the past, but developers quickly broke the API in newer versions.

---

_Comment by @zanieb on 2025-07-23 01:30_

This is sort of resolved with https://github.com/astral-sh/uv/pull/12763 but might need some extra design to work in a shebang

---

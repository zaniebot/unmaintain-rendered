---
number: 17228
title: Enforce uv version in pyproject.toml
type: issue
state: closed
author: yshiftanlightricks
labels:
  - enhancement
assignees: []
created_at: 2025-12-23T13:21:03Z
updated_at: 2026-01-04T19:07:08Z
url: https://github.com/astral-sh/uv/issues/17228
synced_at: 2026-01-07T13:12:19-06:00
---

# Enforce uv version in pyproject.toml

---

_Issue opened by @yshiftanlightricks on 2025-12-23 13:21_

### Summary

Our project can be built with uv lock only when using uv version 0.9.0 or later.
We would really appreciate the ability to enforce the uv version in pyproject.toml.
I understand that if someone already has an older version installed, they might not get a clear error message.
However, at least for new installations or newer versions, this would be very helpful.


### Example

_No response_

---

_Label `enhancement` added by @yshiftanlightricks on 2025-12-23 13:21_

---

_Comment by @sebadevo on 2025-12-23 14:41_

to check the uv version you can add the following in your pyproject.toml
```toml
[tool.uv]
required-version = ">=0.9.0"
```

---

_Closed by @charliermarsh on 2026-01-04 19:06_

---

_Comment by @charliermarsh on 2026-01-04 19:07_

Yeah, `required-version` is the trick here. You can do `==0.9.0` too if needed.

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 19:07_

---

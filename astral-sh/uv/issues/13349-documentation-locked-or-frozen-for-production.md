---
number: 13349
title: "Documentation: `--locked` or `--frozen` for production?"
type: issue
state: closed
author: thomthom
labels:
  - question
assignees: []
created_at: 2025-05-08T17:13:52Z
updated_at: 2025-05-08T19:04:38Z
url: https://github.com/astral-sh/uv/issues/13349
synced_at: 2026-01-10T01:25:32Z
---

# Documentation: `--locked` or `--frozen` for production?

---

_Issue opened by @thomthom on 2025-05-08 17:13_

### Question

This page, "Using uv in Docker" uses `--locked` in its examples: https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs

![Image](https://github.com/user-attachments/assets/4ed4f8ec-1bfe-49ab-a131-8fbe1cb6fe41)

Whereas the docker example repo uses `--frozen`: https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs

![Image](https://github.com/user-attachments/assets/75f682d1-d444-446a-ae16-68f030345960)

It's not clear to me exactly when to use what. What should one use in production?

Tangent to the first example, when is `--no-editable` useful?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @thomthom on 2025-05-08 17:13_

---

_Comment by @zanieb on 2025-05-08 17:34_

The Docker repo just hasn't been updated. `--locked` will enforce the `pyproject.toml` matches the `uv.lock` — `--frozen` will only read the `uv.lock` and ignore the `pyproject.toml`. The former usually matches expectations better.

`--no-editable` is useful if you want to remove the project source directory, i.e., if you are shipping the environment itself somewhere.

---

_Comment by @thomthom on 2025-05-08 19:03_

Thank you for the response. 👍

I guess this one can be closed then. If its the docker repo that needs a change to match the docs?

---

_Comment by @zanieb on 2025-05-08 19:04_

Yep! 

---

_Closed by @zanieb on 2025-05-08 19:04_

---

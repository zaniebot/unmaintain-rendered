```yaml
number: 13416
title: Include Build Dependencies in the Lock File
type: issue
state: closed
author: pythonweb2
labels:
  - enhancement
assignees: []
created_at: 2025-05-12T21:00:48Z
updated_at: 2025-05-13T02:56:52Z
url: https://github.com/astral-sh/uv/issues/13416
synced_at: 2026-01-12T16:01:28Z
```

# Include Build Dependencies in the Lock File

---

_@pythonweb2_

### Summary

Right now, as I understand it, uv locks all of the dependencies in the pyproject.toml file. However, from what I have seen, there is no way to lock build dependencies for all of those dependencies.

For example, I add `pydantic` to my dependencies, which depends on `pydantic-core`, which has a build dependency of `maturin`. When I use `uv sync -U --refresh` and disable wheels, then when uv builds pydantic from source, it will pull the version of maturin specified (right now `"maturin>=1,<2"`), and this could break source builds in different pipelines, etc.

It seems like to achieve a truly locked environment; these dependencies would need to be locked as well. Would it be possible to include that in the lock file?

### Example

_No response_

---

_Label `enhancement` added by @pythonweb2 on 2025-05-12 21:00_

---

_Comment by @zanieb on 2025-05-12 21:23_

We're working on this.

---

_Assigned to @Gankra by @zanieb on 2025-05-12 21:23_

---

_Comment by @pythonweb2 on 2025-05-12 21:24_

Ok, thanks for the quick response! Feel free to close this as a duplicate after linking to the associated work items.

---

_Comment by @charliermarsh on 2025-05-13 02:56_

I believe it's #5190.

---

_Closed by @charliermarsh on 2025-05-13 02:56_

---

---
number: 17027
title: Document how to disable dev dependency installation in Docker builds
type: issue
state: closed
author: aeroyorch
labels:
  - enhancement
assignees: []
created_at: 2025-12-08T10:10:34Z
updated_at: 2025-12-09T18:12:09Z
url: https://github.com/astral-sh/uv/issues/17027
synced_at: 2026-01-07T13:12:19-06:00
---

# Document how to disable dev dependency installation in Docker builds

---

_Issue opened by @aeroyorch on 2025-12-08 10:10_

### Summary

Hi!

In the Docker best practices guide’s [Optimizations section](https://docs.astral.sh/uv/guides/integration/docker/#optimizations), it would be helpful to document how to skip dev dependencies during production builds through:

```dockerfile
ENV UV_NO_DEV=1
```

Or:

`uv sync --no-dev`

A short subsection explaining when and how to use these options in Dockerfiles would improve clarity for production scenarios.

Thanks, and great work with uv! 🚀

---

_Label `enhancement` added by @aeroyorch on 2025-12-08 10:10_

---

_Referenced in [astral-sh/uv#17030](../../astral-sh/uv/pulls/17030.md) on 2025-12-08 13:06_

---

_Closed by @zanieb on 2025-12-09 18:12_

---

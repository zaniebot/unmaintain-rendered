```yaml
number: 14666
title: "`uv cache prune --ci` removes all useful cache"
type: issue
state: closed
author: MarthinusBosman
labels:
  - question
assignees: []
created_at: 2025-07-16T17:22:15Z
updated_at: 2025-07-16T19:12:27Z
url: https://github.com/astral-sh/uv/issues/14666
synced_at: 2026-01-12T16:01:54Z
```

# `uv cache prune --ci` removes all useful cache

---

_@MarthinusBosman_

### Summary

I'm not sure what the internal logic is for the `uv cache prune --ci` command, but running it locally as well as within an azure devops pipeline causes all packages to have to be redownloaded.

Perhaps some explanation on the logic is needed, but I definitely think this is a bug.

To reproduce:
1. Delete `.venv/` folder.
2. Run `uv sync` observe files downloaded/fetched from cache.
3. Run `uv cache prune --ci`
4. Delete `.venv/` folder
5. Run `uv sync` and observe all packages need to be downloaded from scratch.

Running just `uv cache prune` does not cause this.

### Platform

Windows 11 | Ubuntu

### Version

0.7.19

### Python version

Python 3.12.0

---

_Label `bug` added by @MarthinusBosman on 2025-07-16 17:22_

---

_Comment by @zanieb on 2025-07-16 17:26_

See https://github.com/astral-sh/uv/pull/5391

---

_Comment by @zanieb on 2025-07-16 17:27_

and https://docs.astral.sh/uv/concepts/cache/#caching-in-continuous-integration

---

_Label `bug` removed by @zanieb on 2025-07-16 17:27_

---

_Label `question` added by @zanieb on 2025-07-16 17:27_

---

_Closed by @zanieb on 2025-07-16 19:12_

---

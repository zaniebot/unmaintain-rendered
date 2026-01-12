```yaml
number: 15442
title: How to check the pinned version of ruff as used by uv format
type: issue
state: open
author: nth10sd
labels:
  - question
assignees: []
created_at: 2025-08-22T00:31:56Z
updated_at: 2025-08-22T00:33:23Z
url: https://github.com/astral-sh/uv/issues/15442
synced_at: 2026-01-12T16:02:11Z
```

# How to check the pinned version of ruff as used by uv format

---

_@nth10sd_

### Question

In `uv format`, how can we check the pinned version? I understand we can select the specific version via `uv format --version SOME_RUFF_VERSION`, but it would be nice to be able to see which ruff version is used/pinned under `uv format --help`, for example.

Alternatively, we could have a config option for `uv format` to always use the latest and greatest ruff version. E.g., the current pinned ruff version is 0.12.5, whereas the latest ruff version on PyPI is 0.12.10, and we should be able to always select the latest (0.12.10) in this case. (Subject to some possible upper bound since uv will be the same version)

### Platform

All

### Version

0.8.13

---

_Label `question` added by @nth10sd on 2025-08-22 00:31_

---

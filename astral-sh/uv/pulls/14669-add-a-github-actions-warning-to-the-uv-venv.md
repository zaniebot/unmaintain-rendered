```yaml
number: 14669
title: "Add a GitHub Actions warning to the `uv venv` behavior change"
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/venv-warn-ci
created_at: 2025-07-16T18:06:42Z
updated_at: 2025-07-17T22:20:22Z
url: https://github.com/astral-sh/uv/pull/14669
synced_at: 2026-01-12T16:11:20Z
```

# Add a GitHub Actions warning to the `uv venv` behavior change

---

_@zanieb_

In an attempt to increase the visibility of this warning in CI users, emit it as a GitHub Actions annotation. This begins establishing general infrastructure for emitting CI provider-specific warnings.

We don't need this for 0.8, it's more important for when this change is breaking.

---

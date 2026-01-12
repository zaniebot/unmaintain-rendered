```yaml
number: 4366
title: "Remove extraneous installations in `uv sync` by default"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: ibraheem/sync-exact
created_at: 2024-06-17T18:28:12Z
updated_at: 2024-06-17T18:38:35Z
url: https://github.com/astral-sh/uv/pull/4366
synced_at: 2026-01-12T16:06:11Z
```

# Remove extraneous installations in `uv sync` by default

---

_@ibraheemdev_

## Summary

First step of https://github.com/astral-sh/uv/issues/4358. `uv sync` will now remove any extraneous installations by default.

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-17 18:28_

---

_Label `preview` added by @ibraheemdev on 2024-06-17 18:28_

---

_@zanieb approved on 2024-06-17 18:30_

---

_Comment by @ibraheemdev on 2024-06-17 18:38_

As a reference point, Poetry makes this opt-*in*, with `poetry install --sync`. However, exact sync by default with an opt-out seems like the correct default for `uv sync`.

> When you omit the `--sync` option, you can install any subset of optional groups without removing those that are already installed. This is very useful, for example, in multi-stage Docker builds, where you run poetry install multiple times in different build stages.

---

_Merged by @ibraheemdev on 2024-06-17 18:38_

---

_Closed by @ibraheemdev on 2024-06-17 18:38_

---

_Branch deleted on 2024-06-17 18:38_

---

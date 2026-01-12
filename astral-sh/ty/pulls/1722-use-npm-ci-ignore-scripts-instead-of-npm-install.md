```yaml
number: 1722
title: "Use `npm ci --ignore-scripts` instead of `npm install`"
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/npm
created_at: 2025-12-02T15:01:22Z
updated_at: 2025-12-02T15:14:57Z
url: https://github.com/astral-sh/ty/pull/1722
synced_at: 2026-01-12T15:54:27Z
```

# Use `npm ci --ignore-scripts` instead of `npm install`

---

_@woodruffw_

## Summary

This uses `npm ci --ignore-scripts` instead of `npm install`, which is more hermetic/reproducible and also forbids build-time script execution (which none of the deps in SchemaStore should need).

See https://github.com/astral-sh/ruff/pull/21742 and https://github.com/astral-sh/uv/pull/16915 for equivalent changes.

## Test Plan

Identical to the uv change, which I tested locally 🙂 

---

_Assigned to @woodruffw by @woodruffw on 2025-12-02 15:01_

---

_@MichaReiser approved on 2025-12-02 15:12_

---

_Merged by @woodruffw on 2025-12-02 15:14_

---

_Closed by @woodruffw on 2025-12-02 15:14_

---

_Branch deleted on 2025-12-02 15:14_

---

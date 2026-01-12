```yaml
number: 12818
title: "Replace `--frozen` with `--locked` in Docker integration guide"
type: pull_request
state: merged
author: johnthagen
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-04-10T18:12:10Z
updated_at: 2025-04-10T19:58:29Z
url: https://github.com/astral-sh/uv/pull/12818
synced_at: 2026-01-12T16:10:24Z
```

# Replace `--frozen` with `--locked` in Docker integration guide

---

_@johnthagen_

## Summary

Replace `--frozen` with `--locked` in Docker integration guide. `--locked` additionally validates that `uv.lock` is "fresh"/up to date, which will catch errors if the user accidentally updated `pyproject.toml` but did not run `uv lock` before building the container. This is probably a better/safer default to recommend to users to avoid surprising/incorrect behavior.

## References

- External guides already recommend using `--locked` instead of `--frozen`
  - https://hynek.me/articles/docker-uv/
- @zanieb seemed to indicate they might agree that `--locked` would be better to avoid surprises
  - https://github.com/astral-sh/uv/issues/10793#issuecomment-2743956736

## Test Plan

Used `--locked` in `uv` Python projects using Docker and validated that it works as expected.

---

_Review requested from @zanieb by @Gankra on 2025-04-10 19:44_

---

_@Gankra approved on 2025-04-10 19:45_

This looks reasonable, but I'll give zanieb a chance to veto it.

---

_Comment by @Gankra on 2025-04-10 19:47_

Actually rereading the discussion I think this is just right to land.

---

_Merged by @Gankra on 2025-04-10 19:47_

---

_Closed by @Gankra on 2025-04-10 19:47_

---

_Branch deleted on 2025-04-10 19:58_

---

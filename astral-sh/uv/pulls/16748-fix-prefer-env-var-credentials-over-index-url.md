```yaml
number: 16748
title: "fix: prefer env var credentials over index URL username; guard cache & git-store; add tests"
type: pull_request
state: open
author: MarthinusBosman
labels: []
assignees: []
base: main
head: MarthinusBosman/issue16733
created_at: 2025-11-15T17:05:43Z
updated_at: 2025-11-18T15:31:21Z
url: https://github.com/astral-sh/uv/pull/16748
synced_at: 2026-01-10T05:58:11Z
```

# fix: prefer env var credentials over index URL username; guard cache & git-store; add tests

---

_Pull request opened by @MarthinusBosman on 2025-11-15 17:05_

## Summary

Ensure environment credentials take precedence when resolving private registry credentials. When both an index URL and environment variables (e.g. `UV_INDEX_PRIVATE_REGISTRY_USERNAME` / `UV_INDEX_PRIVATE_REGISTRY_PASSWORD`) provide credentials, prefer the environment variables and avoid overwriting cached credentials with URL-derived username. Adds unit tests for the precedence behavior and updates docs/references. Closes #16733.

## Test Plan

- Added unit tests that assert env username/password override credentials derived from an index URL.
- Ran `cargo nextest run` locally; all tests pass.

---

_Review requested from @zanieb by @konstin on 2025-11-18 15:31_

---

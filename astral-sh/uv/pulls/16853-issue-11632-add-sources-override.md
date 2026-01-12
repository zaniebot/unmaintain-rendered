```yaml
number: 16853
title: Issue 11632 - Add sources override
type: pull_request
state: open
author: mawildoer
labels: []
assignees: []
draft: true
base: main
head: mawildoer/issue-11632
created_at: 2025-11-25T23:14:58Z
updated_at: 2025-11-25T23:18:08Z
url: https://github.com/astral-sh/uv/pull/16853
synced_at: 2026-01-12T16:12:28Z
```

# Issue 11632 - Add sources override

---

_@mawildoer_

## Summary

This PR adds support for defining dependency sources in `uv.toml` files, enabling local development overrides without modifying the committed `pyproject.toml`. This addresses a common pain point
where developers need to temporarily override package sources for local development across multiple coupled projects.

### Changes

Allow `sources` in `uv.toml`: Removed the validation error that previously prevented the `[sources]` table in `uv.toml` files

### Motivation

When working on interdependent packages during local development, developers often need to point package sources to local editable installations. Previously, this required modifying
`pyproject.toml`, which makes it difficult to manage dev dependencies.

### Solution

With this change, developers can create a local `uv.toml` file (which can be added to `.gitignore`) to override sources:

```toml
# uv.toml (local, not committed)
[sources]
my-package = { path = "../my-package", editable = true }
httpx = { git = "https://github.com/encode/httpx", branch = "dev" }
```

When both uv.toml and pyproject.toml define sources for the same package, the uv.toml definition takes precedence. This allows clean local overrides while keeping the committed configuration
unchanged.

### Test Plan

1. Install a dependency set with `uv`, including a dependency with a source
2. Add a new source to the dependency set in `uv.toml`
3. Verify that the dependency is installed from the new source
4. Verify this works with transitive dependencies

Closes #11632
Closes #15895

---

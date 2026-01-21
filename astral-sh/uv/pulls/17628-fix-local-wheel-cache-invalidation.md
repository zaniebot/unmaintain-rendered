```yaml
number: 17628
title: Fix local wheel cache invalidation
type: pull_request
state: open
author: danilohorta
labels: []
assignees: []
base: main
head: fix-local-wheel-cache-invalidation
created_at: 2026-01-20T15:23:54Z
updated_at: 2026-01-21T10:46:48Z
url: https://github.com/astral-sh/uv/pull/17628
synced_at: 2026-01-21T11:00:13Z
```

# Fix local wheel cache invalidation

---

_@danilohorta_

Fixes #16617

## Problem

When using `uv run --with path/to/wheel.whl`, rebuilding the wheel file doesn't invalidate the cached environment. Users must run `uv cache prune` to see changes from a rebuilt wheel.

## Root Cause

The `CachedEnvironment` creates a cache key from only the resolution (distribution paths) and interpreter hash. It doesn't account for file modification times of local wheels, so when a file at the same path is modified, the cache key remains unchanged.

## Solution

Include file modification times (mtime) in the cache hash for local distributions:

- Added `local_distribution_timestamps()` helper to extract mtimes for local wheels and source archives
- Combined resolution hash with timestamp hash for the final cache key
- Excluded directory timestamps to avoid false cache invalidation (directories change frequently for unrelated reasons)
- Added integration test `run_with_local_wheel_rebuild` to verify the fix

## Changes

**Modified files:**
- `crates/uv/src/commands/project/environment.rs` - Core cache logic
- `crates/uv/tests/it/run.rs` - Integration test

## Testing

- ✅ New integration test passes
- ✅ All run module tests pass
- ✅ No performance impact on registry-based dependencies
- ✅ Properly invalidates cache when local wheels are rebuilt

## Performance Considerations

- Minimal overhead: one `fs::metadata()` call per local file distribution
- No impact on remote dependencies
- OS metadata is heavily cached

---

_Comment by @zanieb on 2026-01-20 18:05_

Was this authored by using an LLM? Could you disclose how you decided on this design and iterated on the fix?

---

_Comment by @danilohorta on 2026-01-21 10:46_

I did use LLM tools to iterate through the solution space and draft/validate alternative approaches, but the investigation, design choice, and code changes were mine. My first iteration was to disable venv caching whenever local files/projects were present, but that was too broad for my use case. As a middle ground, I now fold file timestamps for local wheel/source archive files into the env hash, so rebuilding a local wheel at the same path invalidates the cache while keeping caching on for the rest.

I’m intentionally not including local project directories yet because a directory’s mtime is a noisy proxy: it can change for incidental file churn (temp files, build outputs, IDE caches, etc.) and may not change for content-only edits. This change only tracks concrete files. If you think it’s worth covering local projects too, I’m happy to discuss other signals (e.g., a targeted file list like pyproject.toml/lockfiles, or content hashing).

---

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
updated_at: 2026-01-20T15:23:54Z
url: https://github.com/astral-sh/uv/pull/17628
synced_at: 2026-01-20T15:44:17Z
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

```yaml
number: 17484
title: Update system-configuration dependency to 0.7.0 to fix sandbox panic
type: issue
state: closed
author: andersrennermalm
labels: []
assignees: []
created_at: 2026-01-15T14:45:41Z
updated_at: 2026-01-15T14:46:45Z
url: https://github.com/astral-sh/uv/issues/17484
synced_at: 2026-01-15T15:50:27Z
```

# Update system-configuration dependency to 0.7.0 to fix sandbox panic

---

_@andersrennermalm_

## Problem

`uv run` panics when executed inside a macOS sandbox (Claude Code, OpenAI Codex) with:

```
thread 'main2' panicked at system-configuration-0.6.1/src/dynamic_store.rs:154:1:
Attempted to create a NULL object.
```

## Root Cause

uv 0.9.25 uses `system-configuration` crate version **0.6.1**, which panics when the sandbox blocks access to `com.apple.SystemConfiguration.configd`.

## Solution

The fix has been merged and released in `system-configuration` **0.7.0**:
- PR: https://github.com/mullvad/system-configuration-rs/pull/59
- Issue: https://github.com/mullvad/system-configuration-rs/issues/71

Please update the `system-configuration` dependency from 0.6.1 to 0.7.0.

## Related Issues

- #16664 - macOS: `uv run` panics inside SystemConfiguration when sandboxed
- #16916 - `uv init` fails when called from Claude Code's sandbox on macOS

## How to Verify the Fix

Users can test if a new uv version works in sandboxed environments:

```bash
# In Claude Code or other macOS sandbox:
uv run python -c "print('sandbox test passed')"
```

If it prints `sandbox test passed` without panicking, the fix is working.

## Environment

- macOS 14+ (Sonoma)
- uv 0.9.25 (Homebrew 2026-01-13)
- Claude Code sandbox (seatbelt)


---

_Comment by @konstin on 2026-01-15 14:46_

Duplicate of #16916

---

_Closed by @konstin on 2026-01-15 14:46_

---

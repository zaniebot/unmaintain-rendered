```yaml
number: 3239
title: Add --suppress-error flag for permission-denied handling
type: pull_request
state: closed
author: M1tsumi
labels: []
assignees: []
base: master
head: suppress-error-permission
created_at: 2025-12-06T20:51:28Z
updated_at: 2025-12-06T21:35:21Z
url: https://github.com/BurntSushi/ripgrep/pull/3239
synced_at: 2026-01-12T18:23:15Z
```

# Add --suppress-error flag for permission-denied handling

---

_@M1tsumi_

This PR introduces a new `--suppress-error` flag that lets users control how ripgrep handles permission-denied errors during file traversal and searching.

The new flag has two modes:
- `--suppress-error=permission` - suppresses the error output but still exits with code 2
- `--suppress-error=nopermission` - suppresses output AND treats these files as skipped (exit code unaffected)

## Changes

**Flag and enum** (`lowargs.rs`, `defs.rs`, `hiargs.rs`)
- Added `SuppressErrorMode` enum with `PermissionSilence` and `PermissionSkip` variants
- Hooked it up through the low-args → hi-args pipeline

- Added `handle_search_error()` helper used by both single-threaded and parallel search
- Checks error kind against suppression mode to decide whether to print/set error flag

**Tests**
- Flag parsing tests in `defs.rs`
- Unit tests for the error handler covering mode × error type combinations
- Existing tests pass

This leaves the Default behavior unchanged - new behavior only kicks in if you pass the flag.

---

_Comment by @BurntSushi on 2025-12-06 21:12_

ripgrep already has a `--no-messages` flag.

---

_Closed by @BurntSushi on 2025-12-06 21:12_

---

_Comment by @ltrzesniewski on 2025-12-06 21:16_

Wow, you even left the prompt in the PR /s

---

_Branch deleted on 2025-12-06 21:35_

---

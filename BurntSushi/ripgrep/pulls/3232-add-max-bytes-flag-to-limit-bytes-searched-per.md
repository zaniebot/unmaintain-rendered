```yaml
number: 3232
title: Add --max-bytes flag to limit bytes searched per file
type: pull_request
state: open
author: manascb1344
labels: []
assignees: []
base: master
head: feat/max-bytes
created_at: 2025-12-01T08:49:00Z
updated_at: 2025-12-24T06:26:18Z
url: https://github.com/BurntSushi/ripgrep/pull/3232
synced_at: 2026-01-12T18:23:15Z
```

# Add --max-bytes flag to limit bytes searched per file

---

_@manascb1344_

This PR adds a new `--max-bytes NUM` flag that tells ripgrep to only read and search the first `NUM` bytes of each file or stream. This is useful when matches are expected only in a small header region (for example HTTP headers in large cache files), and can greatly reduce I/O.

- New flag: `--max-bytes NUM` (per file/stream)
- Applied consistently to mmap, regular files, stdin, and multiline searches
- Multiline preallocation is capped by `min(NUM, file_size)` when `--max-bytes` is set
- Plays well with `--max-count 1` for fast “find first hit in header” use cases
- Includes tests for normal files, stdin, and binary-looking files

Implements the feature requested in [#3035](https://github.com/BurntSushi/ripgrep/issues/3035).

---

_Comment by @manascb1344 on 2025-12-01 09:00_

@BurntSushi 
All tests and CI checks are now passing for this PR. When you have a moment, could you please review and consider merging the --max-bytes flag change?


---

_Comment by @manascb1344 on 2025-12-24 06:26_

@BurntSushi  Any updates on this?

---

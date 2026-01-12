```yaml
number: 3224
title: Separate flags from paths in decompression tool invocations
type: pull_request
state: open
author: jafd
labels: []
assignees: []
base: master
head: fix-decompress-tool-flags
created_at: 2025-11-20T10:13:54Z
updated_at: 2025-11-20T10:13:54Z
url: https://github.com/BurntSushi/ripgrep/pull/3224
synced_at: 2026-01-12T18:23:15Z
```

# Separate flags from paths in decompression tool invocations

---

_@jafd_

This fixes the bug when the filename of a compressed file starts with the `-` character and it gets passed to the decompression tool, but gets treated as an option instead of file.

In the best case, the option would be invalid and the decompression tool would finish with an error. In the worst case, the filename that looks like a valid option could potentially overwrite or remove data as a side effect.

All decompression tools in use here follow the convention that everything after `--` is treated as a filename.

---

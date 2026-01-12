```yaml
number: 3210
title: Add POSIX character class support to globset
type: pull_request
state: open
author: matanshavit
labels: []
assignees: []
base: master
head: issue-2962-posix-character-classes
created_at: 2025-10-28T23:01:04Z
updated_at: 2025-10-28T23:01:04Z
url: https://github.com/BurntSushi/ripgrep/pull/3210
synced_at: 2026-01-12T18:23:15Z
```

# Add POSIX character class support to globset

---

_@matanshavit_

Implement support for POSIX character classes like [:space:], [:digit:], [:alpha:], etc. in glob patterns. All 12 standard POSIX classes are supported: alnum, alpha, blank, cntrl, digit, graph, lower, print, punct, space, upper, and xdigit.

The implementation uses ASCII-only definitions to avoid locale-dependent behavior. POSIX classes can be used alone or combined with regular character ranges (e.g., [[:digit:]a-f] for hexadecimal digits).

Fixes #2962

---

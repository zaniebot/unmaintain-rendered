```yaml
number: 1871
title: "core: Add field-match-separator and field-context-separator"
type: pull_request
state: closed
author: Anthuang
labels:
  - rollup
assignees: []
base: master
head: separators
created_at: 2021-05-26T01:46:04Z
updated_at: 2021-06-01T01:51:26Z
url: https://github.com/BurntSushi/ripgrep/pull/1871
synced_at: 2026-01-12T18:23:14Z
```

# core: Add field-match-separator and field-context-separator

---

_@Anthuang_

This allows for the customization of the match and context separators. The match separator defaults to `:`, while the context separator defaults to `-`. Both are limited to a single byte.

Closes #1842

---

_Label `rollup` added by @BurntSushi on 2021-05-31 01:00_

---

_Comment by @BurntSushi on 2021-05-31 01:00_

Thanks! I'm bringing this in via my rollup branch. I added tests and also removed the one byte restriction. The printer itself supports any number of bytes (including zero), so we might as well allow that.

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

```yaml
number: 3116
title: "--cursor-ignore ripgrep option"
type: pull_request
state: closed
author: Zackh1998
labels: []
assignees: []
base: master
head: zh/cursor-ignore-option
created_at: 2025-07-31T06:11:04Z
updated_at: 2025-07-31T07:28:54Z
url: https://github.com/BurntSushi/ripgrep/pull/3116
synced_at: 2026-01-12T18:23:15Z
```

# --cursor-ignore ripgrep option

---

_@Zackh1998_

Adds a --cursor-ignore option for ripgrep. It behaves the same as --ignore-file, except it acts as a strict filter.

--ignore-file is lower precedence than --glob, so glob matches can match contents that are normally .gitignored.

The --cursor-ignore option blocks .cursorignore options even if a --glob arg would match them.

---

_Closed by @Zackh1998 on 2025-07-31 07:24_

---

_Branch deleted on 2025-07-31 07:24_

---

_Branch restored on 2025-07-31 07:28_

---

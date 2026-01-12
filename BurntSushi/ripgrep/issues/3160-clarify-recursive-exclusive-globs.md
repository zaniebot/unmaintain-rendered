```yaml
number: 3160
title: clarify recursive, exclusive globs
type: issue
state: closed
author: mcandre
labels: []
assignees: []
created_at: 2025-09-24T23:57:51Z
updated_at: 2025-10-11T00:39:00Z
url: https://github.com/BurntSushi/ripgrep/issues/3160
synced_at: 2026-01-12T16:13:25Z
```

# clarify recursive, exclusive globs

---

_@mcandre_

The documented exclusion `--glob <dir>/*` examples use explicit asterisks (`*`).

Are directory exclusions automatically recursive?

What is the syntax to ignore _both_ the directory and all recursive contents?

Can we please make the (recursive) star implicit (e.g. `--glob <dir>`)

---

_Comment by @BurntSushi on 2025-09-25 00:10_

Please provide an MRE of current behavior versus what you want/expect.

---

_Locked by @ghost on 2025-10-11 00:39_

---

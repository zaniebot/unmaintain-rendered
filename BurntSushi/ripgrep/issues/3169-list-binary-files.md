```yaml
number: 3169
title: list binary files
type: issue
state: closed
author: 3052
labels: []
assignees: []
created_at: 2025-10-05T17:40:39Z
updated_at: 2025-10-06T13:43:17Z
url: https://github.com/BurntSushi/ripgrep/issues/3169
synced_at: 2026-01-12T16:13:25Z
```

# list binary files

---

_@3052_

is it possible to just get a list of binary files in the current tree, example types

JAR
PNG
7Z
RAR
ZIP

---

_Comment by @3052 on 2025-10-05 17:46_

this seems to work
```
rg -a -l \x00
```

---

_Locked by @ghost on 2025-10-06 13:43_

---

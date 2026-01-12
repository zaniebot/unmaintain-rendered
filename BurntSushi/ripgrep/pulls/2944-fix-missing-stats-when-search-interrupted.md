```yaml
number: 2944
title: fix missing stats when search interrupted
type: pull_request
state: closed
author: yorkart
labels:
  - rollup
assignees: []
base: master
head: bugfix/stats
created_at: 2024-12-03T14:20:19Z
updated_at: 2025-09-20T01:08:38Z
url: https://github.com/BurntSushi/ripgrep/pull/2944
synced_at: 2026-01-12T18:23:14Z
```

# fix missing stats when search interrupted

---

_@yorkart_

When `-m` is specified, the `bytes searched` part of  stats is missing or small.

eg:
command： 
```bash
rg --stats -m 10 -e 'keyword' /path/to/file
```

output
```bash
...
(10 lines)
...

10 matches
10 matched lines
1 files contained matches
1 files searched
4535 bytes printed
0 bytes searched   ------------------------>  (the field is missing)
0.001751 seconds spent searching
0.003299 seconds
```

---

_@BurntSushi approved on 2025-08-17 14:47_

Thank you! Nice find!

---

_Label `rollup` added by @BurntSushi on 2025-08-17 14:47_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

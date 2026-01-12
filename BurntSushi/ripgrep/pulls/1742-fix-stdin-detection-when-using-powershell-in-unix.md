```yaml
number: 1742
title: "Fix stdin detection when using Powershell in Unix (fix #1741)"
type: pull_request
state: closed
author: r-darwish
labels: []
assignees: []
base: master
head: pwsh-unix-stdin
created_at: 2020-11-23T06:25:37Z
updated_at: 2020-11-23T18:47:17Z
url: https://github.com/BurntSushi/ripgrep/pull/1742
synced_at: 2026-01-12T18:23:14Z
```

# Fix stdin detection when using Powershell in Unix (fix #1741)

---

_@r-darwish_

It seems that Powershell uses sockets instead of FIFOs to redirect the output between commands

---

_Closed by @BurntSushi on 2020-11-23 15:24_

---

_Comment by @BurntSushi on 2020-11-23 15:24_

Thank you so much! I really appreciate you digging into this and fixing it. :-)

---

_Branch deleted on 2020-11-23 18:47_

---

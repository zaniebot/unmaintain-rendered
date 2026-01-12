```yaml
number: 1237
title: "searcher: add option to disable BOM sniffing"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/disable-bom-sniff
created_at: 2019-04-06T13:20:53Z
updated_at: 2019-04-06T14:35:09Z
url: https://github.com/BurntSushi/ripgrep/pull/1237
synced_at: 2026-01-12T18:23:13Z
```

# searcher: add option to disable BOM sniffing

---

_@BurntSushi_

This commit adds a new encoding feature where the -E/--encoding flag
will now accept a value of 'none'. When given this value, all encoding
related machinery is disabled and ripgrep will search the raw bytes of
the file, including the BOM if it's present.

Closes #1207, Closes #1208

---

_Merged by @BurntSushi on 2019-04-06 14:35_

---

_Closed by @BurntSushi on 2019-04-06 14:35_

---

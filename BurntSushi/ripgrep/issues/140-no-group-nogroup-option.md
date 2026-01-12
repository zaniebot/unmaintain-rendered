```yaml
number: 140
title: "--no-group / --nogroup option"
type: issue
state: closed
author: brson
labels: []
assignees: []
created_at: 2016-09-30T23:31:20Z
updated_at: 2016-10-03T20:20:04Z
url: https://github.com/BurntSushi/ripgrep/issues/140
synced_at: 2026-01-12T18:23:11Z
```

# --no-group / --nogroup option

---

_@brson_

In `ag` the `--nogroup` option changes the output format so that the file name is prepended to every match line, instead of grouping matches under a file 'header' and blank line. This saves a lot of vertical space, and I'm accustomed to using it when my results run off the screen.


---

_Comment by @BurntSushi on 2016-09-30 23:34_

Does `--no-heading` do the trick?


---

_Comment by @brson on 2016-10-03 20:20_

Yep!


---

_Closed by @brson on 2016-10-03 20:20_

---

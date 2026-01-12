```yaml
number: 1180
title: Is it possible to use ripgrep to search for file bigger than a certain size?
type: issue
state: closed
author: miguelmota
labels: []
assignees: []
created_at: 2019-01-29T00:38:11Z
updated_at: 2019-01-29T00:55:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1180
synced_at: 2026-01-12T16:13:23Z
```

# Is it possible to use ripgrep to search for file bigger than a certain size?

---

_@miguelmota_

For example, use ripgrep to return a list of all files recursively under a directory that are equal to or larger than a certain size ie 1GB

Wondering if it's doable
thanks

---

_Comment by @BurntSushi on 2019-01-29 00:55_

Nope. Use a different tool, like `find`, to do that and pipe the list of files to search to ripgrep with xargs.

---

_Closed by @BurntSushi on 2019-01-29 00:55_

---

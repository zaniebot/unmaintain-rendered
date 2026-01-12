```yaml
number: 961
title: use buffered writing for single threaded search
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/use-buffer
created_at: 2018-06-24T00:21:19Z
updated_at: 2018-06-24T00:49:09Z
url: https://github.com/BurntSushi/ripgrep/pull/961
synced_at: 2026-01-12T18:23:13Z
```

# use buffered writing for single threaded search

---

_@BurntSushi_

This also uses a buffer for `--files` where it didn't previously, so this will likely speed that up too when there are many file paths to print.

---

_Merged by @BurntSushi on 2018-06-24 00:49_

---

_Closed by @BurntSushi on 2018-06-24 00:49_

---

_Branch deleted on 2018-06-24 00:49_

---

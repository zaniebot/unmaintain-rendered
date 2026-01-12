```yaml
number: 61
title: Allow options before --help and --version
type: issue
state: closed
author: snocl
labels: []
assignees: []
created_at: 2016-09-24T12:21:12Z
updated_at: 2016-09-24T15:46:22Z
url: https://github.com/BurntSushi/ripgrep/issues/61
synced_at: 2026-01-12T18:23:11Z
```

# Allow options before --help and --version

---

_@snocl_

Currently `rg argument --version` will work fine, but `rg -u --version` will fail with `Invalid arguments.`.

This feature would be nice, since it would make adding default options with CLI aliases easier (e.g. `alias rg='rg --no-mmap'` or `alias rg='rg -u'`). Currently these aliases break `--help` and `--version`.

(Tested with `rg --version` reporting `0.1.17`.)


---

_Comment by @0xmohit on 2016-09-24 14:07_

Dup of #47


---

_Comment by @snocl on 2016-09-24 15:46_

Ah, thank you, closing. :)


---

_Closed by @snocl on 2016-09-24 15:46_

---

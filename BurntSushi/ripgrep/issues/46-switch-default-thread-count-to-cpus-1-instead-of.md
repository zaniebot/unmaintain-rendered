```yaml
number: 46
title: "switch default thread count to `cpus - 1` instead of `cpus`"
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-24T04:06:05Z
updated_at: 2016-09-25T19:02:39Z
url: https://github.com/BurntSushi/ripgrep/issues/46
synced_at: 2026-01-12T18:23:11Z
```

# switch default thread count to `cpus - 1` instead of `cpus`

---

_@BurntSushi_

It makes sense to not use one thread more than the number of CPUs you have. Namely, `-j` launches `N` workers for searching, while the main thread does directory traversal.

Ideally, `cpus` would be the number of physical cores (i.e., not counting hyper threading).


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 04:06_

---

_Comment by @BurntSushi on 2016-09-24 04:09_

Maybe a better approach here would be to subtract `1` from `-j`, since that might match more closely to what users expect (and would actually be accurate, since `-j` is short for `--threads`).


---

_Closed by @BurntSushi on 2016-09-25 19:02_

---

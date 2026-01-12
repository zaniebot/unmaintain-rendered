```yaml
number: 296
title: Add swig type
type: issue
state: closed
author: mcepl
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2016-12-29T23:52:04Z
updated_at: 2016-12-30T02:01:10Z
url: https://github.com/BurntSushi/ripgrep/issues/296
synced_at: 2026-01-12T16:13:21Z
```

# Add swig type

---

_@mcepl_

That’s [swig](http://swig.org/) which uses `*.i` and `*.def` files. Equivalent of
```
--type-add 'swig:*.i' --type-add 'swig:*.def'
```

---

_Label `enhancement` added by @BurntSushi on 2016-12-30 00:18_

---

_Label `help wanted` added by @BurntSushi on 2016-12-30 00:18_

---

_Comment by @BurntSushi on 2016-12-30 00:19_

A PR adding it would be most welcome. It should be a simple change to this list: https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/types.rs#L82

---

_Closed by @BurntSushi on 2016-12-30 02:01_

---

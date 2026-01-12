```yaml
number: 194
title: search from stdin?
type: issue
state: closed
author: hh9527
labels: []
assignees: []
created_at: 2016-10-24T12:09:59Z
updated_at: 2016-10-24T12:29:48Z
url: https://github.com/BurntSushi/ripgrep/issues/194
synced_at: 2026-01-12T18:23:11Z
```

# search from stdin?

---

_@hh9527_

Is there flags to support grep content from stdin?
like `set | grep ABC`


---

_Comment by @BurntSushi on 2016-10-24 12:29_

Umm, try it?

```
$ echo hi | rg hi
1:hi
```

Note that if you're on Windows, there is currently an open bug (#94). You can work around it by explicitly searching stdin `rg hi -`.


---

_Closed by @BurntSushi on 2016-10-24 12:29_

---

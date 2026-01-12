```yaml
number: 1302
title: improve error message when preprocessing binary could not be found
type: issue
state: closed
author: phiresky
labels:
  - bug
assignees: []
created_at: 2019-06-16T21:54:38Z
updated_at: 2019-06-16T23:05:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1302
synced_at: 2026-01-12T16:13:23Z
```

# improve error message when preprocessing binary could not be found

---

_@phiresky_

#### What version of ripgrep are you using?
ripgrep 11.0.1


`ripgrep --pre=nonexist hi`

currently prints

```
textfile: No such file or directory (os error 2)
otherfile: No such file or directory (os error 2)
```

which looks like those files are non existent, even though it is in fact the preprocessing binary that is missing. It would be great if you could improve the error message to show e.g. `Could not find preprocessing binary nonexist`.

Reference: https://github.com/phiresky/ripgrep-all/issues/4 https://github.com/phiresky/ripgrep-all/issues/6 

---

_Renamed from "Improve error message" to "improve error message when preprocessing binary could not be found" by @BurntSushi on 2019-06-16 22:24_

---

_Label `bug` added by @BurntSushi on 2019-06-16 23:02_

---

_Closed by @BurntSushi on 2019-06-16 23:05_

---

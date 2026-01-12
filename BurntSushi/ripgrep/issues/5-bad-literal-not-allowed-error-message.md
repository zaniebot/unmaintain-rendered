```yaml
number: 5
title: "bad \"literal not allowed\" error message"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2016-09-14T01:20:20Z
updated_at: 2018-01-12T09:43:42Z
url: https://github.com/BurntSushi/ripgrep/issues/5
synced_at: 2026-01-12T16:13:21Z
```

# bad "literal not allowed" error message

---

_@BurntSushi_

Example:

```
$ rg '\n'
Literal '
' not allowed.
```

It should probably look like this instead:

```
$ rg '\n'
Literal '\n' not allowed.
```


---

_Label `bug` added by @BurntSushi on 2016-09-14 01:20_

---

_Closed by @BurntSushi on 2016-09-16 22:12_

---

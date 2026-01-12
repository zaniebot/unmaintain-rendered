```yaml
number: 1695
title: support for pager invocation
type: issue
state: closed
author: cehteh
labels:
  - duplicate
assignees: []
created_at: 2020-10-01T14:14:01Z
updated_at: 2020-10-01T14:22:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1695
synced_at: 2026-01-12T16:13:24Z
```

# support for pager invocation

---

_@cehteh_

#### Describe your feature request

I use a wrapper around ripgrep to feed the output into 'less'.

```
cat /usr/local/bin/rgp
#!/bin/bash
rg -p "$@" | less -SR
```

Asking around, other people do so as well.

It would be nice if such a feature could be somehow integrated into rg itself. Either by a $PAGER env var or by a --pager option. Or chosen automatically when stdout 'isatty()' and a suitable pager is found.









---

_Comment by @BurntSushi on 2020-10-01 14:22_

Dupe of #86 

---

_Closed by @BurntSushi on 2020-10-01 14:22_

---

_Label `duplicate` added by @BurntSushi on 2020-10-01 14:22_

---

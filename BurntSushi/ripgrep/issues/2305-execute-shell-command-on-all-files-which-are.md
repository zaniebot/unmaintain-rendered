```yaml
number: 2305
title: execute shell command on all files which are matched
type: issue
state: closed
author: makarono
labels:
  - wontfix
assignees: []
created_at: 2022-09-13T15:31:30Z
updated_at: 2022-09-13T15:39:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2305
synced_at: 2026-01-12T16:13:24Z
```

# execute shell command on all files which are matched

---

_@makarono_

#### Execute arbitrary shell command

It would be nice feature if when using -l option could execute command on those files

example:

```
rg -l test -X sed -i 's:test:foo:g' {}
```

same thing as fd find does with -X option and gnu-find with --exec option



---

_Comment by @BurntSushi on 2022-09-13 15:39_

I think this is beyond the scope of ripgrep. I'd encourage you to use `xargs` or some other mechanism.

---

_Closed by @BurntSushi on 2022-09-13 15:39_

---

_Label `wontfix` added by @BurntSushi on 2022-09-13 15:39_

---

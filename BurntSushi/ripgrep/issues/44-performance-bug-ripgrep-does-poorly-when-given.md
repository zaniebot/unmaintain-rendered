```yaml
number: 44
title: "performance bug: ripgrep does poorly when given lots directory arguments"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2016-09-24T03:28:46Z
updated_at: 2016-10-30T01:22:04Z
url: https://github.com/BurntSushi/ripgrep/issues/44
synced_at: 2026-01-12T18:23:11Z
```

# performance bug: ripgrep does poorly when given lots directory arguments

---

_@BurntSushi_

ripgrep is doing some non-trivial work for every file searched in positional parameters, and there's really no reason it should.


---

_Label `bug` added by @BurntSushi on 2016-09-24 03:28_

---

_Comment by @BurntSushi on 2016-09-24 23:49_

In the `github.com/rust-lang/rust` repo, there is a pretty big time difference:

```
$ time rg E0401 | wc -l
7

real    0m0.072s
user    0m0.393s
sys     0m0.087s
$ time rg E0401 src/lib* | wc -l
6

real    0m0.202s
user    0m1.623s
sys     0m0.007s
```

If `-u` is passed to the second command, then the time difference goes away.

This means `ripgrep` is spending a lot of time reinitializing ignore state. There are some obvious things we shouldn't be redoing, like the globs and type filters from the command line, but those happen even with the `-u` flag, so they aren't the problem. The likely problem is re-traversing parent directories for `gitignore` files. Indeed, if `--no-ignore-parent` is given, then the performance difference goes away.

This means we need to keep our gitignores from parent directories around and try to reuse them.


---

_Renamed from "performance bug: ripgrep does poorly when given lots of positional arguments" to "performance bug: ripgrep does poorly when given lots directory arguments" by @BurntSushi on 2016-10-29 00:28_

---

_Comment by @BurntSushi on 2016-10-30 01:22_

Fixed in #202.


---

_Closed by @BurntSushi on 2016-10-30 01:22_

---

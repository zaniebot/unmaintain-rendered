```yaml
number: 479
title: "--glob not processing absolute paths correctly"
type: issue
state: closed
author: Olie440
labels: []
assignees: []
created_at: 2017-05-10T10:23:32Z
updated_at: 2017-05-10T13:09:54Z
url: https://github.com/BurntSushi/ripgrep/issues/479
synced_at: 2026-01-12T16:13:22Z
```

# --glob not processing absolute paths correctly

---

_@Olie440_

Glob appears to calculating all paths relatively even if the start with `/`:
![image](https://cloud.githubusercontent.com/assets/6021283/25894174/bec21a2e-3572-11e7-9ddd-639498782efc.png)

OS: MacOS 10.12.4 (darwin 16.5.0)
RipGrep version: ripgrep 0.5.1

---

_Comment by @BurntSushi on 2017-05-10 10:48_

I don't think this is a bug. I don't see how to implement this behavior. Notice that `grep` has the same behavior:

```
$ tree
.
└── foo

0 directories, 1 file
$ cat foo
test
$ rg test -g '!/tmp/ripgrep-479/test'
foo
1:test
$ grep -r test --exclude /tmp/ripgrep-479/test
foo:test
```

Perhpas this is a documentation bug.

---

_Closed by @Olie440 on 2017-05-10 13:09_

---

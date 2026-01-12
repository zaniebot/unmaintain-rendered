```yaml
number: 1547
title: "grep-cli: Support files compressed by compress(1)"
type: pull_request
state: closed
author: mineo
labels:
  - rollup
assignees: []
base: master
head: archive-uncompress
created_at: 2020-04-10T09:27:14Z
updated_at: 2020-05-09T09:05:18Z
url: https://github.com/BurntSushi/ripgrep/pull/1547
synced_at: 2026-01-12T18:23:14Z
```

# grep-cli: Support files compressed by compress(1)

---

_@mineo_

While Linux distributions (at least Arch Linux, RHEL, Debian) do not support
compressing files with compress(1), macOS & AIX do (the utility is part of POSIX). Additionally, gzip is able
to uncompress such compressed files and provides an `uncompress` binary.

---

_Label `rollup` added by @BurntSushi on 2020-05-09 02:19_

---

_@BurntSushi approved on 2020-05-09 02:19_

Thanks!

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---

_Branch deleted on 2020-05-09 09:05_

---

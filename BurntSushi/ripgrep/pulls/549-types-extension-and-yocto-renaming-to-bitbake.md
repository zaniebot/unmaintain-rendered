```yaml
number: 549
title: Types extension and Yocto renaming to BitBake
type: pull_request
state: merged
author: lilianmoraru
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-07-08T12:48:10Z
updated_at: 2017-08-23T21:52:28Z
url: https://github.com/BurntSushi/ripgrep/pull/549
synced_at: 2026-01-12T18:23:13Z
```

# Types extension and Yocto renaming to BitBake

---

_@lilianmoraru_

Yocto -> BitBake: because `bitbake` is just one tool of the many tools that the `Yocto` project has and these files are specific to it.
GNUmakefile: the `make` tool is case-sensitive on this one
*.prf: is basically the equivalent of `*.pri`, just that one is included through `include(file)`(where you have `file.pri`) and another is included through `CONFIG += file`(where you have `file.prf` usually in `mkspecs`).

---

_Merged by @BurntSushi on 2017-08-23 21:52_

---

_Closed by @BurntSushi on 2017-08-23 21:52_

---

_Comment by @BurntSushi on 2017-08-23 21:52_

Seems good, thanks!

---

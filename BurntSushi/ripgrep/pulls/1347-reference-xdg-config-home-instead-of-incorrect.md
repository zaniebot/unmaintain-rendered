```yaml
number: 1347
title: reference XDG_CONFIG_HOME instead of incorrect XDG_CONFIG_DIR
type: pull_request
state: merged
author: lousyd
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2019-08-09T16:44:44Z
updated_at: 2019-08-09T17:37:38Z
url: https://github.com/BurntSushi/ripgrep/pull/1347
synced_at: 2026-01-12T18:23:13Z
```

# reference XDG_CONFIG_HOME instead of incorrect XDG_CONFIG_DIR

---

_@lousyd_

A couple of comments in ignore/src/gitignore.rs referenced "XDG_CONFIG_DIR", which isn't an XDG variable. I believe that "XDG_CONFIG_HOME" is what was meant.

---

_@BurntSushi approved on 2019-08-09 17:37_

That's right, definitely. Thanks!

---

_Merged by @BurntSushi on 2019-08-09 17:37_

---

_Closed by @BurntSushi on 2019-08-09 17:37_

---

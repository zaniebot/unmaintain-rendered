```yaml
number: 1118
title: Add postscript to known file types.
type: pull_request
state: merged
author: anntzer
labels: []
assignees: []
merged: true
base: master
head: postscript
created_at: 2018-11-22T22:10:06Z
updated_at: 2018-11-23T14:49:48Z
url: https://github.com/BurntSushi/ripgrep/pull/1118
synced_at: 2026-01-12T18:23:13Z
```

# Add postscript to known file types.

---

_@anntzer_

Although postscript/encapsulated postscript is usually thought of as a
binary format, it's actually mostly ASCII, so ripgrep will not ignore
these files.

The situation is basically the same as for pdf, which is also already
present in the list of known filetypes.

---

_@BurntSushi approved on 2018-11-23 14:45_

Makes sense, thanks!

---

_Merged by @BurntSushi on 2018-11-23 14:46_

---

_Closed by @BurntSushi on 2018-11-23 14:46_

---

_Branch deleted on 2018-11-23 14:49_

---

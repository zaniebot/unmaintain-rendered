```yaml
number: 406
title: Add new -M/--max-columns option.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: longlines-finish
created_at: 2017-03-13T01:11:16Z
updated_at: 2017-03-13T08:57:10Z
url: https://github.com/BurntSushi/ripgrep/pull/406
synced_at: 2026-01-12T18:23:13Z
```

# Add new -M/--max-columns option.

---

_@BurntSushi_

This permits setting the maximum line width with respect to the number
of bytes in a line. Omitted lines (whether part of a match, replacement
or context) are replaced with a message stating that the line was
elided.

Fixes #129

---

_Comment by @BurntSushi on 2017-03-13 01:11_

cc @RalfJung 

---

_Merged by @BurntSushi on 2017-03-13 01:21_

---

_Closed by @BurntSushi on 2017-03-13 01:21_

---

_Branch deleted on 2017-03-13 01:21_

---

_Comment by @RalfJung on 2017-03-13 08:57_

I have to say I preferred the look of this with a space between the colon and the "line omitted" mark, but oh well. Thanks for adopting my PR :)

---

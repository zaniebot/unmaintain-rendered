```yaml
number: 3008
title: "doc(FAQ): add option -d for xargs in example"
type: pull_request
state: closed
author: sharpchen
labels: []
assignees: []
base: master
head: faq-xargs
created_at: 2025-03-05T23:21:22Z
updated_at: 2025-08-17T20:52:58Z
url: https://github.com/BurntSushi/ripgrep/pull/3008
synced_at: 2026-01-12T18:23:14Z
```

# doc(FAQ): add option -d for xargs in example

---

_@sharpchen_

_No description provided._

---

_Comment by @BurntSushi on 2025-08-17 20:52_

This doesn't really make sense in the broader context of the prose. If you continue reading, it suggests an even more robust solution than `-d '\n'` by using the NUL terminator. That works in the presence of any kind of whitespace.

---

_Closed by @BurntSushi on 2025-08-17 20:52_

---

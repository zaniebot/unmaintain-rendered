```yaml
number: 3092
title: add  the ID field
type: pull_request
state: closed
author: melroy12
labels: []
assignees: []
base: master
head: fix
created_at: 2025-07-05T23:31:26Z
updated_at: 2025-08-19T20:33:14Z
url: https://github.com/BurntSushi/ripgrep/pull/3092
synced_at: 2026-01-12T18:23:15Z
```

# add  the ID field

---

_@melroy12_

solves the #3081

---

_Comment by @BurntSushi on 2025-08-19 20:33_

This isn't how I'd want to go about this unfortunately. What I'm willing to do is attached a monotonically increasing integer to each glob (starting at `0`), similar to how `regex-automata` treats multi-pattern regexes. But attaching arbitrary strings imposes extra costs for everyone _and_ requires users of the library to opt into it.

---

_Closed by @BurntSushi on 2025-08-19 20:33_

---

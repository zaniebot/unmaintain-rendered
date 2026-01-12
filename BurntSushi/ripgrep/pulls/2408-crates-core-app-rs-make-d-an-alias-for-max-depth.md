```yaml
number: 2408
title: "crates/core/app.rs: Make \"-D\" an alias for \"--max-depth\""
type: pull_request
state: closed
author: jurajlutter
labels: []
assignees: []
base: master
head: master
created_at: 2023-02-01T08:24:30Z
updated_at: 2023-07-07T18:27:24Z
url: https://github.com/BurntSushi/ripgrep/pull/2408
synced_at: 2026-01-12T18:23:14Z
```

# crates/core/app.rs: Make "-D" an alias for "--max-depth"

---

_@jurajlutter_

For better usability (at least for me), add an alias "-D" for the "--max-depth" option.

---

_Comment by @jurajlutter on 2023-05-27 10:01_

A very friendly "ping"?

---

_Comment by @BurntSushi on 2023-05-27 11:37_

My initial instinct here is to say no, because the number of available short flags remaining is very small. I'm not convinced this flag is common enough to warrant one of those remaining short flags.

---

_Closed by @BurntSushi on 2023-07-07 18:27_

---

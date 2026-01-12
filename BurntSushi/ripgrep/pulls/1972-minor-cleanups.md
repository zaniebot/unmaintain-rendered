```yaml
number: 1972
title: Minor cleanups
type: pull_request
state: closed
author: a1346054
labels: []
assignees: []
base: master
head: master
created_at: 2021-08-14T16:30:25Z
updated_at: 2023-07-08T18:56:06Z
url: https://github.com/BurntSushi/ripgrep/pull/1972
synced_at: 2026-01-12T18:23:14Z
```

# Minor cleanups

---

_@a1346054_

_No description provided._

---

_Comment by @BurntSushi on 2023-07-08 18:25_

This looks like a weird PR. There might be one or two things here that seem okay, but most of it looks either wrong or just changing things for the sake of changing them.

---

_Closed by @BurntSushi on 2023-07-08 18:25_

---

_Comment by @a1346054 on 2023-07-08 18:56_

I don't remember what tooling I used 2 years ago, but these days `shellcheck` detects some of the script problems (they always need to be reviewed by a human) and `shfmt` gives guidance on unified coding style for the scripts.

The trailing whitespace removal should be self-explanatory.

---

```yaml
number: 620
title: Fix printing context after an early return from a search
type: pull_request
state: merged
author: Detegr
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-10-01T17:00:26Z
updated_at: 2017-10-08T12:10:36Z
url: https://github.com/BurntSushi/ripgrep/pull/620
synced_at: 2026-01-12T18:23:13Z
```

# Fix printing context after an early return from a search

---

_@Detegr_

Print the context if there's a context left to be printed after returning early from a search (because of `--max-count`).

Fixes #402.

---

_Comment by @Detegr on 2017-10-01 17:03_

Added a couple of test cases as well that now work the same way as `grep` does.

---

_@BurntSushi approved on 2017-10-08 12:10_

Nice find and fix! Thanks for the regression tests too, much appreciated. :-)

---

_Merged by @BurntSushi on 2017-10-08 12:10_

---

_Closed by @BurntSushi on 2017-10-08 12:10_

---

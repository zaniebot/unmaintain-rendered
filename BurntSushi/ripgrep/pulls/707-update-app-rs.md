```yaml
number: 707
title: Update app.rs
type: pull_request
state: merged
author: flip111
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2017-12-04T14:30:09Z
updated_at: 2017-12-30T21:04:22Z
url: https://github.com/BurntSushi/ripgrep/pull/707
synced_at: 2026-01-12T18:23:13Z
```

# Update app.rs

---

_@flip111_

Clarified `--ignore-file` to address https://github.com/BurntSushi/ripgrep/issues/684

---

_Review comment by @BurntSushi on `src/app.rs`:426 on 2017-12-04 14:45_

`s/pattern/patterns`

---

_Review comment by @BurntSushi on `src/app.rs`:427 on 2017-12-04 14:45_

`.rgignore` is technically deprecated, although it may be resurfacing. For now, this should say `.ignore` instead of `.rgignore`.

---

_@BurntSushi requested changes on 2017-12-04 14:46_

Thanks! Just a couple of nits. :-)

---

_@okdana reviewed on 2017-12-04 15:05_

---

_Review comment by @okdana on `src/app.rs`:432 on 2017-12-04 15:05_

precedence\*

---

_@BurntSushi approved on 2017-12-30 21:03_

---

_Merged by @BurntSushi on 2017-12-30 21:04_

---

_Closed by @BurntSushi on 2017-12-30 21:04_

---

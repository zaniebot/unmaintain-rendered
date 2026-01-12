```yaml
number: 1236
title: partially migrate to using byte strings
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/partial-bstr-migration
created_at: 2019-04-06T02:30:29Z
updated_at: 2019-04-06T03:24:09Z
url: https://github.com/BurntSushi/ripgrep/pull/1236
synced_at: 2026-01-12T18:23:13Z
```

# partially migrate to using byte strings

---

_@BurntSushi_

This migrates a few of ripgrep's core crates to using byte strings, which let's us get rid of a fair bit of annoying platform specific code (and is now pushed down into `bstr`).

This PR also fixes a performance regression that occurs in some cases when `-w` is used. Namely, literal detection is thwarted in some cases.

---

_Merged by @BurntSushi on 2019-04-06 03:24_

---

_Closed by @BurntSushi on 2019-04-06 03:24_

---

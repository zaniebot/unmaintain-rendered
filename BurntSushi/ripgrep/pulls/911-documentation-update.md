```yaml
number: 911
title: Documentation Update
type: pull_request
state: merged
author: gsquire
labels: []
assignees: []
merged: true
base: master
head: update-ignore-docs
created_at: 2018-05-04T03:37:51Z
updated_at: 2018-05-07T16:32:48Z
url: https://github.com/BurntSushi/ripgrep/pull/911
synced_at: 2026-01-12T18:23:13Z
```

# Documentation Update

---

_@gsquire_

A small documentation update for `OverrideBuilder` and `GitignoreBuilder`.

Closes #910 

---

_Review comment by @BurntSushi on `ignore/src/overrides.rs`:144 on 2018-05-04 11:05_

I think we can be more precise here:

```
/// When this option is changed, only globs added after the change will be affected.
///
/// This is disabled by default.
```

Also, can you add this to the gitignore builder too?

---

_@BurntSushi requested changes on 2018-05-04 11:05_

Thanks! Just a couple of nits.

---

_@BurntSushi approved on 2018-05-06 23:02_

Thanks!

---

_Merged by @BurntSushi on 2018-05-06 23:03_

---

_Closed by @BurntSushi on 2018-05-06 23:03_

---

_Branch deleted on 2018-05-07 16:32_

---

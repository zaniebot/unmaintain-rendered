```yaml
number: 223
title: Add parallel recursive directory iterator.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ignore-parallel
created_at: 2016-11-06T01:31:50Z
updated_at: 2016-11-06T02:19:49Z
url: https://github.com/BurntSushi/ripgrep/pull/223
synced_at: 2026-01-12T18:23:12Z
```

# Add parallel recursive directory iterator.

---

_@BurntSushi_

This adds a new walk type in the `ignore` crate, `WalkParallel`, which
provides a way for recursively iterating over a set of paths in parallel
while respecting various ignore rules.

The API is a bit strange, as a closure producing a closure isn't
something one often sees, but it does seem to work well.

This also allowed us to simplify much of the worker logic in ripgrep
proper, where MultiWorker is now gone.

---

_Merged by @BurntSushi on 2016-11-06 02:19_

---

_Closed by @BurntSushi on 2016-11-06 02:19_

---

_Branch deleted on 2016-11-06 02:19_

---

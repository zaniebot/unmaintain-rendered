```yaml
number: 5439
title: "Split `ResolutionGraph::from_state` into methods"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/resolution-graph-refactorings
created_at: 2024-07-25T10:33:35Z
updated_at: 2024-07-25T12:40:11Z
url: https://github.com/astral-sh/uv/pull/5439
synced_at: 2026-01-10T13:37:23Z
```

# Split `ResolutionGraph::from_state` into methods

---

_Pull request opened by @konstin on 2024-07-25 10:33_

Two changes split out from the instability work:

* Break `ResolutionGraph::from_state` into methods before adding new logic to it.
* `ResolutionGraph`: Convert `NodeKey` type to `PackageRef` struct: Another small refactoring to make subsequent changes easier.

No functional changes.

@BurntSushi I hope this doesn't interfere with your work too much, the `PackageRef` should at least make debugging panics here easier.

---

_Label `internal` added by @konstin on 2024-07-25 10:33_

---

_Review requested from @BurntSushi by @konstin on 2024-07-25 10:33_

---

_@BurntSushi approved on 2024-07-25 11:57_

I like it!

And nope, actually shouldn't impact my changes at all. All of mine are (so far) in the resolver.

---

_Merged by @konstin on 2024-07-25 12:40_

---

_Closed by @konstin on 2024-07-25 12:40_

---

_Branch deleted on 2024-07-25 12:40_

---

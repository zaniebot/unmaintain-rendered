```yaml
number: 9334
title: "uv-resolver: add new `UniversalMarker` type"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/add-universal-marker
created_at: 2024-11-21T19:27:49Z
updated_at: 2024-11-23T15:59:57Z
url: https://github.com/astral-sh/uv/pull/9334
synced_at: 2026-01-10T12:00:00Z
```

# uv-resolver: add new `UniversalMarker` type

---

_Pull request opened by @BurntSushi on 2024-11-21 19:27_

This effectively combines a PEP 508 marker and an as-yet-specified
marker for expressing conflicts among extras and groups.

This just defines the type and threads it through most of the various
points in the code that previously used `MarkerTree` only. Some parts
do still continue to use `MarkerTree` specifically, e.g., when dealing
with non-universal resolution or exporting to `requirements.txt`.

This doesn't change any behavior.

There are some other smaller commits in this PR will smaller changes,
so I'd recommend reviewing commit-by-commit.

Ref #9289


---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-21 19:27_

---

_Comment by @BurntSushi on 2024-11-22 13:21_

@charliermarsh Since this is just a refactoring PR, I'm going to bring this in. If you leave any feedback, I'm happy to follow-up!

---

_Merged by @BurntSushi on 2024-11-22 13:21_

---

_Closed by @BurntSushi on 2024-11-22 13:21_

---

_Branch deleted on 2024-11-22 13:21_

---

_Label `internal` added by @BurntSushi on 2024-11-23 15:59_

---

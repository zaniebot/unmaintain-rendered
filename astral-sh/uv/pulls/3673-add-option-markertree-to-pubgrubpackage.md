```yaml
number: 3673
title: "add `Option<MarkerTree>` to `PubGrubPackage`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/pubgrub-package-marker
created_at: 2024-05-20T18:04:52Z
updated_at: 2024-05-20T23:56:25Z
url: https://github.com/astral-sh/uv/pull/3673
synced_at: 2026-01-10T14:32:20Z
```

# add `Option<MarkerTree>` to `PubGrubPackage`

---

_Pull request opened by @BurntSushi on 2024-05-20 18:04_

I'm plucking this off my WIP of resolver forking since it grew to
a fairly sizable change on its own. In this PR, all we do is add
`Option<MarkerTree>` to `PubGrubPackage`, but otherwise don't do any
semantic changes.

While conceptually simple, adding this tripped some test failures as a
result of the output depending on hashmap iteration order. So part of
this PR is devoted to removing that dependency and making the output
independent of hashmap iteration order.

I took the approach of making `PubGrubPackage` implement `Ord`. This in
turn required implementing `Ord` for a number of other types. So this
feels like a somewhat invasive change, although I did feel like all of
the `Ord` impls "made sense" for the types they were being added to.

Reviewing this PR commit-by-commit should be much easier than looking
at the full diff.

Closes #3359


---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-20 18:04_

---

_@charliermarsh approved on 2024-05-20 18:36_

This all looks reasonable to me. Thanks for splitting it out.

---

_Label `internal` added by @charliermarsh on 2024-05-20 18:36_

---

_Merged by @BurntSushi on 2024-05-20 23:56_

---

_Closed by @BurntSushi on 2024-05-20 23:56_

---

_Branch deleted on 2024-05-20 23:56_

---

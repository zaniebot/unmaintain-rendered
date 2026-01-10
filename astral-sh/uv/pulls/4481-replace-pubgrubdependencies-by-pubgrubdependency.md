```yaml
number: 4481
title: "Replace `PubGrubDependencies` by `PubGrubDependency`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/PubGrubDependencies
created_at: 2024-06-24T17:58:03Z
updated_at: 2024-06-25T22:11:54Z
url: https://github.com/astral-sh/uv/pull/4481
synced_at: 2026-01-10T13:48:28Z
```

# Replace `PubGrubDependencies` by `PubGrubDependency`

---

_Pull request opened by @konstin on 2024-06-24 17:58_

In the last PR (#4430), we flatten the requirements. In the next PR (#4435), we want to pass `Url` around next to `PubGrubPackage` and `Range<Version>` to keep track of which `Requirement`s added a url across forking. This PR is a refactoring split out from #4435 that rolls the dependency conversion into a single iterator and introduces a new `PubGrubDependency` struct as abstraction over `(PubGrubPackage, Range<Version>)` (or `(PubGrubPackage, Range<Version>, VerbatimParsedUrl)` in the next PR), and it removes the now unnecessary `PubGrubDependencies` abstraction.


---

_Label `internal` added by @konstin on 2024-06-24 17:58_

---

_@BurntSushi approved on 2024-06-25 14:12_

Yes. Much better. Thank you!

---

_Marked ready for review by @konstin on 2024-06-25 14:14_

---

_Merged by @konstin on 2024-06-25 22:11_

---

_Closed by @konstin on 2024-06-25 22:11_

---

_Branch deleted on 2024-06-25 22:11_

---

```yaml
number: 12742
title: Test publishing with pypa/gh-action-pypi-publish
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/publishing-with-the-official-gh-action
created_at: 2025-04-08T12:27:54Z
updated_at: 2025-04-25T16:27:42Z
url: https://github.com/astral-sh/uv/pull/12742
synced_at: 2026-01-10T11:10:40Z
```

# Test publishing with pypa/gh-action-pypi-publish

---

_Pull request opened by @konstin on 2025-04-08 12:27_

A publish testing for #11652

---

_Label `internal` added by @konstin on 2025-04-08 12:28_

---

_Marked ready for review by @konstin on 2025-04-08 12:48_

---

_Converted to draft by @konstin on 2025-04-08 12:48_

---

_Comment by @konstin on 2025-04-08 14:05_

We have to see if that check is robust enough with parallel PRs, but test coverage for this would be nice.

---

_Marked ready for review by @konstin on 2025-04-08 14:05_

---

_Review requested from @Gankra by @zanieb on 2025-04-08 17:41_

---

_@Gankra approved on 2025-04-08 18:02_

...i feel like i'm missing a lot of context on this

---

_Comment by @Gankra on 2025-04-08 18:04_

re: parallel PRs, you could also publish a prerelease-or-whatever with a uuid-ish component?

---

_Review requested from @zanieb by @konstin on 2025-04-09 10:19_

---

_Comment by @konstin on 2025-04-09 10:36_

The change is an addition to the existing `uv publish` testing script to also test the pypa github publish action for #11652. I tried to keep the new test similar to the existing test script with the same version bumping strategy, even though it has less coverage by not testing our skip-existing strategy.

---

_Comment by @konstin on 2025-04-25 16:27_

I going ahead with merging this because I think it makes sense to have coverage for this, it's easy enough to remove if the test causes more false positives than it's worth.

---

_Merged by @konstin on 2025-04-25 16:27_

---

_Closed by @konstin on 2025-04-25 16:27_

---

_Branch deleted on 2025-04-25 16:27_

---

```yaml
number: 6159
title: "tests: fix marker emitting tests"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-tests
created_at: 2024-08-16T17:55:01Z
updated_at: 2024-08-19T13:02:31Z
url: https://github.com/astral-sh/uv/pull/6159
synced_at: 2026-01-10T13:09:50Z
```

# tests: fix marker emitting tests

---

_Pull request opened by @BurntSushi on 2024-08-16 17:55_

The test output seems to depend on using Python 3.12.1 specifically.
While I'm not sure how it happens, it seems like these can get out of
sync between CI and local testing. In this case, I had a problem where
the marker expressions emitted locally were tied to Python 3.12.4, but
the tests in CI were tied to Python 3.12.1. Changing the test to require
3.12.1 specifically fixes this.

---

_@zanieb approved on 2024-08-16 17:57_

---

_@charliermarsh approved on 2024-08-16 19:59_

---

_Renamed from "tests: update snapshots and delete unreferenced snapshots" to "tests: fix marker emitting tests" by @BurntSushi on 2024-08-19 12:46_

---

_Comment by @BurntSushi on 2024-08-19 12:48_

I've changed this PR to tweak the failing tests to specifically require Python `3.12.1` instead of just `3.12`. These tests in particular seem sensitive to the specific Python patch version, and this was out of sync between my local environment and CI.

---

_Merged by @BurntSushi on 2024-08-19 13:02_

---

_Closed by @BurntSushi on 2024-08-19 13:02_

---

_Branch deleted on 2024-08-19 13:02_

---

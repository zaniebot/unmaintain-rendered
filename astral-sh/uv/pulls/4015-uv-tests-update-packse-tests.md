```yaml
number: 4015
title: "uv/tests: update packse tests"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/update-packse
created_at: 2024-06-04T16:56:05Z
updated_at: 2024-06-04T17:56:22Z
url: https://github.com/astral-sh/uv/pull/4015
synced_at: 2026-01-12T16:05:59Z
```

# uv/tests: update packse tests

---

_@BurntSushi_

This is just the result of running

    ./scripts/sync_scenarios.sh

From the root of the `uv` repository.

When I initially ran this, it produced some tests with snapshots that
weren't being updated. It turned out this was because the tests weren't
running, as they were gated behind the `python-patch` feature. In this
commit, we add `python-patch` to our `cargo insta` command, which should
update all relevant snapshots.

There are still some superfluous updates as a result of a spell checker
being run on generated files, but 

---

_Review requested from @zanieb by @BurntSushi on 2024-06-04 17:05_

---

_@zanieb approved on 2024-06-04 17:06_

---

_Marked ready for review by @BurntSushi on 2024-06-04 17:56_

---

_Merged by @BurntSushi on 2024-06-04 17:56_

---

_Closed by @BurntSushi on 2024-06-04 17:56_

---

_Branch deleted on 2024-06-04 17:56_

---

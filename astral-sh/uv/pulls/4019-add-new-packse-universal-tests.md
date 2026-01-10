```yaml
number: 4019
title: add new packse universal tests
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/forking-tests
created_at: 2024-06-04T18:01:59Z
updated_at: 2024-06-04T18:25:00Z
url: https://github.com/astral-sh/uv/pull/4019
synced_at: 2026-01-10T13:54:02Z
```

# add new packse universal tests

---

_Pull request opened by @BurntSushi on 2024-06-04 18:01_

This makes some changes to our packse integration scripts to support the new
universal tests. And then runs the scripts to bring them in.

Specifically, the tests being added are from the initial batch added here:
https://github.com/astral-sh/packse/pull/180


---

_@BurntSushi reviewed on 2024-06-04 18:04_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:319 on 2024-06-04 18:04_

Note that this test result is wrong. Notice that `fork-marker-selection-a` is included twice here, with two different versions, but _no_ marker expressions. I believe this needs to be fixed by tracking marker expressions in forked resolver states. Since that isn't trivial to fix, and I'm not sure if there's a good way to mark packse tests as "wrong," I figured it would be best to just get this merged and then fix it in a follow-up.

---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-04 18:04_

---

_Review requested from @zanieb by @BurntSushi on 2024-06-04 18:04_

---

_@zanieb reviewed on 2024-06-04 18:07_

---

_Review comment by @zanieb on `scripts/scenarios/templates/lock.mustache`:62 on 2024-06-04 18:07_

Do you want the package name filters?

```suggestion
        filters => filters
```

---

_@zanieb reviewed on 2024-06-04 18:08_

---

_Review comment by @zanieb on `crates/uv/tests/lock_scenarios.rs`:792 on 2024-06-04 18:08_

Oof these markers are tough on error messages

---

_@zanieb approved on 2024-06-04 18:09_

New scenario setup lgtm! Just the minor note on the filters.

---

_@BurntSushi reviewed on 2024-06-04 18:13_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:792 on 2024-06-04 18:13_

Yeah indeed. We could have an alternative display format that excludes them in error messages, but they also simultaneously seem useful... Not quite sure what to do about that yet.

---

_@BurntSushi reviewed on 2024-06-04 18:13_

---

_Review comment by @BurntSushi on `scripts/scenarios/templates/lock.mustache`:62 on 2024-06-04 18:13_

Nice catch!

---

_Merged by @BurntSushi on 2024-06-04 18:24_

---

_Closed by @BurntSushi on 2024-06-04 18:24_

---

_Branch deleted on 2024-06-04 18:25_

---

```yaml
number: 11323
title: "fix a case of duplicate `torch` packages when using conflicting extras"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/fix-11133
created_at: 2025-02-07T17:19:06Z
updated_at: 2025-02-18T12:47:48Z
url: https://github.com/astral-sh/uv/pull/11323
synced_at: 2026-01-12T16:09:47Z
```

# fix a case of duplicate `torch` packages when using conflicting extras

---

_@BurntSushi_

The underlying cause here, I believe, was that we weren't accounting
for the case where an edge could be visited *without* any extras
enabled. Because of that, we got into situations where we thought there
was only one path to an edge when there were actually more paths. This
in turn lead to us erroneously doing simplification where it actually
isn't justified. And in turn lead to duplicate versions of the same
package being installed in the same environment.

The fix for this ends up being really simple: in the case where we
don't add any conflict items for a package during graph traversal,
we materialize an empty set of conflicts to mark the case of no
extras being enabled when visiting the child edges. This is enough to
propagate the knowledge of multiple paths to the same edge and causes
us to avoid doing improper simplifications.

This does fix the problem in the snapshot, but it does also I think
lead to other cases where simplifications are no longer possible (hence
the changes to the airflow snapshot). But this seems expected, since
we are doing strictly less simplification than we were before. It's
unclear if all of those cases were actual bugs or not though.

The first commit in this PR adds a regression test so that reviewers
can more easily see the diff to the lock file as a result of the fix.

Fixes #11133


---

_@charliermarsh approved on 2025-02-08 01:28_

---

_@charliermarsh reviewed on 2025-02-08 01:33_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock_conflict.rs`:5284 on 2025-02-08 01:33_

I'm having some trouble figuring out why the `2.6.0` version of Airflow doesn't have these on it... I guess it's because that's the "default" version that gets installed if no extras are enabled?

---

_@BurntSushi reviewed on 2025-02-10 14:17_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock_conflict.rs`:5284 on 2025-02-10 14:17_

Yeah I think so. There is a non-optional dependency on `quickpath-airflow-operator`, and the lock entry for that package is this:

```toml
[[package]]
name = "quickpath-airflow-operator"
version = "1.0.2"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "apache-airflow", version = "2.5.0", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-3-pkg-x1'" },
    { name = "apache-airflow", version = "2.6.0", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-3-pkg-x2' or extra != 'extra-3-pkg-x1'" },
]
```

I think that conflict marker on `2.6.0` could be further simplified to `extra != 'extra-3-pkg-x1'`. (A good example of how conflict marker simplification is not perfect.)

---

_Merged by @BurntSushi on 2025-02-10 14:17_

---

_Closed by @BurntSushi on 2025-02-10 14:17_

---

_Branch deleted on 2025-02-10 14:17_

---

_Label `bug` added by @BurntSushi on 2025-02-18 12:47_

---

_Label `lock` added by @BurntSushi on 2025-02-18 12:47_

---

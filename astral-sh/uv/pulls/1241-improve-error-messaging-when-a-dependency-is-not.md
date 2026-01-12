```yaml
number: 1241
title: Improve error messaging when a dependency is not found
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/package-not-found
created_at: 2024-02-03T16:18:26Z
updated_at: 2024-02-05T14:43:06Z
url: https://github.com/astral-sh/uv/pull/1241
synced_at: 2026-01-12T16:04:31Z
```

# Improve error messaging when a dependency is not found

---

_@zanieb_

Previously, whenever we encountered a missing package we would throw an error without information about why the package was requested. This meant that if a transitive dependency required a missing package, the user would have no idea why it was even selected. Here, we track `NotFound` and `NoIndex` errors as `NoVersions` incompatibilities with an attached reason. Improves our test coverage for `--no-index` without `--find-links`.

The [snapshots](https://github.com/astral-sh/puffin/pull/1241/files#diff-3eea1658f165476252f1f061d0aa9f915aabdceafac21611cdf45019447f60ec) show a nice improvement.

I think this will also enable backtracking to another version if some version of transitive dependency has a missing dependency. I'll write a scenario for that next.

Requires https://github.com/zanieb/pubgrub/pull/22

---

_Review requested from @charliermarsh by @zanieb on 2024-02-03 16:32_

---

_Label `error messages` added by @zanieb on 2024-02-03 16:47_

---

_Comment by @zanieb on 2024-02-03 23:17_

I expect this to require some refactoring and polish, but it's ready for review.

---

_Marked ready for review by @zanieb on 2024-02-03 23:17_

---

_@charliermarsh reviewed on 2024-02-04 23:55_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:79 on 2024-02-04 23:55_

Nit: always be available? (I think)

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:86 on 2024-02-05 00:00_

Should we differentiate here that this is for a single _package version_ (like you did for the enums)?

---

_@charliermarsh reviewed on 2024-02-05 00:00_

---

_@charliermarsh reviewed on 2024-02-05 00:01_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:68 on 2024-02-05 00:01_

I tend to make all structs `#[derive(Debug)]` so that I don't have to modify them later when debugging.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:68 on 2024-02-05 00:01_

This one could also be `Clone` and `Copy`.

---

_@charliermarsh reviewed on 2024-02-05 00:01_

---

_@charliermarsh reviewed on 2024-02-05 00:02_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:353 on 2024-02-05 00:02_

Could _perhaps_ be put in a standalone function so that you can use early returns (like `let entry = self.unavailable.get(package_name)?` instead of another level of nesting). But totally up to you, it might end up being more awkward.

---

_@charliermarsh reviewed on 2024-02-05 00:02_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:734 on 2024-02-05 00:02_

This should never happen, right?

---

_@charliermarsh approved on 2024-02-05 00:03_

I think this looks great.

---

_@charliermarsh reviewed on 2024-02-05 00:04_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:734 on 2024-02-05 00:04_

(Wondering if the whole thing should be a debug assert so that we don't have to do this extra hash lookup in release, but what you have here is probably better, it's negligible.)

---

_@zanieb reviewed on 2024-02-05 14:15_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:734 on 2024-02-05 14:15_

Yeah this should never happen. I am just hesitant to panic in production.

---

_Merged by @zanieb on 2024-02-05 14:43_

---

_Closed by @zanieb on 2024-02-05 14:43_

---

_Branch deleted on 2024-02-05 14:43_

---

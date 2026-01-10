```yaml
number: 14511
title: Revert normalization of trailing slashes on index URLs
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/revert-slash
created_at: 2025-07-08T21:56:19Z
updated_at: 2025-07-09T11:50:33Z
url: https://github.com/astral-sh/uv/pull/14511
synced_at: 2026-01-10T06:53:02Z
```

# Revert normalization of trailing slashes on index URLs

---

_Pull request opened by @zanieb on 2025-07-08 21:56_

Reverts:

- #14349
- #14346
- #14245

Retains the test cases. Includes a `find-links` test case.

Supersedes

- https://github.com/astral-sh/uv/pull/14387
- https://github.com/astral-sh/uv/pull/14503

We originally got a report at https://github.com/astral-sh/uv/issues/13707 that inclusion of a trailing slash on an index URL was causing lockfile churn despite having no semantic meaning and resolved the issue by adding normalization that stripped trailing slashes at parse time.

We then discovered that, while there are not semantic differences for trailing slashes on Simple API index URLs, there are differences for some flat (or find links) indexes. As reported in https://github.com/astral-sh/uv/issues/14367, the change in https://github.com/astral-sh/uv/pull/14245 caused a regression for at least one user.

We attempted to fix the regression via a few approaches.

https://github.com/astral-sh/uv/pull/14387 attempted to differentiate between Simple API and flat index URL parsing, but failed to account for the `Deserialize` implementation, which always assumed Simple API-style index URLs and incorrectly trimmed trailing slashes in various cases where we deserialized the `IndexUrl` type from a file. I attempted to resolve this by performing a larger refactor, but it ended up being quite painful. In particular, the `Index` type was a blocker — we don't know the `IndexUrl` variant until we've parsed the `IndexFormat` and having a multi-stage deserializer is not appealing but adding a new intermediate type (i.e., `RawIndex`) is painful due to the pervasiveness of `Index`. Given that we've regressed behavior here and there's not a straight-forward fix, we're reverting the normalization entirely.

https://github.com/astral-sh/uv/pull/14503 attempted to perform normalization at compare-time, but that means we'd fail to invalidate the lockfile when the a trailing slash was added or removed and given that a trailing slash has semantic meaning for a find-links URL... we'd have another correctness problem.

After this revert, we'll retain all index URLs verbatim. The downside to this approach is that we'll be adding a bunch of trailing slashes back to lockfiles that we previously normalized out, and we'll be reverting our fix for users with inconsistent trailing slashes on their index URLs. Users affected by the original motivating issue should use consistent trailing slashes on their URLs, as they do frequently have semantic meaning. We may want to revisit normalization and type aware index URL parsing as part of a larger change. 

Closes  https://github.com/astral-sh/uv/issues/14367




---

_Review requested from @charliermarsh by @zanieb on 2025-07-08 22:18_

---

_Review requested from @jtfmumm by @zanieb on 2025-07-08 22:46_

---

_@charliermarsh approved on 2025-07-08 23:14_

---

_@jtfmumm approved on 2025-07-09 07:08_

---

_Merged by @zanieb on 2025-07-09 11:50_

---

_Closed by @zanieb on 2025-07-09 11:50_

---

_Branch deleted on 2025-07-09 11:50_

---

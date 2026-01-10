```yaml
number: 425
title: "Use `UnusableDependencies` for URL dependency conflicts"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/new-incompat-urls
created_at: 2023-11-14T22:22:15Z
updated_at: 2023-11-17T14:28:13Z
url: https://github.com/astral-sh/uv/pull/425
synced_at: 2026-01-10T15:50:28Z
```

# Use `UnusableDependencies` for URL dependency conflicts

---

_Pull request opened by @zanieb on 2023-11-14 22:22_

Extends #424 with support for URL dependency incompatibilities.

Requires changes to `miette` to prevent URLs from being word wrapped; accepted upstream in https://github.com/zkat/miette/pull/321

---

_Converted to draft by @zanieb on 2023-11-14 22:22_

---

_Comment by @zanieb on 2023-11-15 00:28_

It'd be nice to add a backtracking test case.

---

_@charliermarsh reviewed on 2023-11-15 16:42_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_repeated_url_dependency_version_mismatch.snap`:21 on 2023-11-15 16:42_

I might suggest removing the `and` here, it's a bit awkward on its own line. Maybe we could make this an unordered list? Like prefix each URL with a `-`?

---

_@charliermarsh reviewed on 2023-11-15 16:44_

---

_Review comment by @charliermarsh on `Cargo.toml`:46 on 2023-11-15 16:44_

What's your feeling on waiting for this to merge vs. moving ahead with the fork? (Separately, can we use a full SHA?)

---

_@zanieb reviewed on 2023-11-15 19:47_

---

_Review comment by @zanieb on `Cargo.toml`:46 on 2023-11-15 19:47_

It's been merged now [04f2f26](https://github.com/astral-sh/puffin/pull/425/commits/04f2f26ce65adeabab74412c90e9980e51dc0356)

---

_@zanieb reviewed on 2023-11-15 19:47_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_repeated_url_dependency_version_mismatch.snap`:21 on 2023-11-15 19:47_

I agree. I can look into that.

---

_@zanieb reviewed on 2023-11-15 20:29_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_repeated_url_dependency_version_mismatch.snap`:21 on 2023-11-15 20:29_

Locally:
```
──────────────────────────────────────────────────────────────────────────────────────────────
    3     3 │ 
    4     4 │ ----- stderr -----
    5     5 │   × No solution found when resolving dependencies:
    6     6 │   ╰─▶ root dependencies are unusable: Conflicting URLs for package `werkzeug`:
    7       │-      https://files.pythonhosted.org/packages/bd/24/11c3ea5a7e866bf2d97f0501d0b4b1c9bbeade102bb4b588f0d2919a5212/Werkzeug-2.0.1-py3-none-any.whl
    8       │-      and
    9       │-      https://files.pythonhosted.org/packages/ff/1d/960bb4017c68674a1cb099534840f18d3def3ce44aed12b5ed8b78e0153e/Werkzeug-2.0.0-py3-none-any.whl
          7 │+      - https://files.pythonhosted.org/packages/bd/24/11c3ea5a7e866bf2d97f0501d0b4b1c9bbeade102bb4b588f0d2919a5212/Werkzeug-2.0.1-py3-none-any.whl
          8 │+      - https://files.pythonhosted.org/packages/ff/1d/960bb4017c68674a1cb099534840f18d3def3ce44aed12b5ed8b78e0153e/Werkzeug-2.0.0-py3-none-any.whl
```

Waiting for my Python version nightmare to be fixed to regen the snapshots

---

_@charliermarsh approved on 2023-11-16 00:12_

---

_@zanieb reviewed on 2023-11-16 20:07_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_compile.rs`:50 on 2023-11-16 20:07_

We may want to consider using this elsewhere, but let's start small

---

_Marked ready for review by @zanieb on 2023-11-16 20:07_

---

_@charliermarsh approved on 2023-11-16 20:54_

---

_Merged by @zanieb on 2023-11-17 14:28_

---

_Closed by @zanieb on 2023-11-17 14:28_

---

_Branch deleted on 2023-11-17 14:28_

---

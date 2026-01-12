```yaml
number: 543
title: "Use version in `RegistryIndex`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: registry-index-package-version
created_at: 2023-12-04T14:21:08Z
updated_at: 2023-12-04T19:32:34Z
url: https://github.com/astral-sh/uv/pull/543
synced_at: 2026-01-12T16:04:01Z
```

# Use version in `RegistryIndex`

---

_@konstin_

When building up the `RegistryIndex`, index by both package name and version to fix #537.

---

_@konstin reviewed on 2023-12-04 14:22_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:192 on 2023-12-04 14:22_

I spent too much time trying to this more elegantly in the distribution database before realizing we need to change this later to invalidate non-built wheel anyway so we can do the cheap thing here.

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/plan.rs`:91 on 2023-12-04 14:28_

No, don't think so, I'd suggest to remove this TODO.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:192 on 2023-12-04 14:30_

Why did this file change at all?

---

_@charliermarsh reviewed on 2023-12-04 14:30_

---

_@konstin reviewed on 2023-12-04 14:32_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:192 on 2023-12-04 14:32_

So we don't try to unzip things that are already unzipped and also make correct reports in `install_source_dist_cached`. Without the latter requirement i'd just have put that in the unzipper.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:192 on 2023-12-04 14:36_

Is this unrelated to re-indexing the registry index by version though?

---

_@charliermarsh reviewed on 2023-12-04 14:36_

---

_@charliermarsh reviewed on 2023-12-04 14:43_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:192 on 2023-12-04 14:43_

In other words: can / should this be split into a separate PR?

---

_Comment by @konstin on 2023-12-04 14:59_

Current dependencies on/for this PR:
* main
  * **PR #543** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/543?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #545** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/545?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #548** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/548?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #544** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/544?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/543?utm_source=stack-comment).

---

_@konstin reviewed on 2023-12-04 14:59_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:192 on 2023-12-04 14:59_

The following would fail without the second change, but the tests are passing so i've split this out to #545:

```bash
virtualenv --clear -p 3.12 .venv && RUST_LOG=puffin=debug cargo run -p puffin-cli -- pip-sync --cache-dir cache-sync -v ../ruff-lsp/requirements-dev.txt
```

---

_@charliermarsh reviewed on 2023-12-04 15:05_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/plan.rs`:92 on 2023-12-04 15:05_

Should this be `.last()`, so we take the most recent?

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/plan.rs`:92 on 2023-12-04 15:06_

Oh, you already use `.rev()` in the method.

---

_@charliermarsh reviewed on 2023-12-04 15:06_

---

_@charliermarsh approved on 2023-12-04 15:06_

---

_Merged by @konstin on 2023-12-04 16:26_

---

_Closed by @konstin on 2023-12-04 16:26_

---

_Branch deleted on 2023-12-04 16:26_

---

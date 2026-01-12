```yaml
number: 1822
title: Decouple
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: decouple
created_at: 2023-01-12T17:24:40Z
updated_at: 2023-01-12T19:07:42Z
url: https://github.com/astral-sh/ruff/pull/1822
synced_at: 2026-01-12T05:36:32Z
```

# Decouple

---

_Pull request opened by @not-my-profile on 2023-01-12 17:24_

The first two commits from #1816 so that they can be merged before the maturin packaging situation is resolved.

Please merge via rebase.

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:39_

I am really confused why the second commit makes clippy suddenly complain about dead code ... (see [this failure](https://github.com/charliermarsh/ruff/actions/runs/3904543263/jobs/6670466198)). Anyway once #1816 is merged this command will ignore `ruff_cli` and then clippy apparently is happy.

---

_@not-my-profile reviewed on 2023-01-12 17:39_

---

_@charliermarsh reviewed on 2023-01-12 17:41_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:41_

I think it has to do with the fact that the WASM build uses `commands.rs`, but never `cache.rs` based on the codepaths. I've run into this before.

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:43_

Does it work to just move the `allow` into `cache.rs`?

---

_@charliermarsh reviewed on 2023-01-12 17:43_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:43_

Right as I said #1816 will move `cache` out of `ruff` to `ruff_cli` so this will no longer be an issue. For the time being the `-A dead_code` should make the CI go through.

---

_@not-my-profile reviewed on 2023-01-12 17:43_

---

_@not-my-profile reviewed on 2023-01-12 17:44_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:44_

Let me check.

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:45_

Yeah totally. I'd just rather put it in the file if it's equally easy. But not a big deal.

---

_@charliermarsh reviewed on 2023-01-12 17:45_

---

_@not-my-profile reviewed on 2023-01-12 17:49_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:49_

Fixed by adding `#![cfg_attr(target_family = "wasm", allow(dead_code))]` to `cache.rs` and `diagnostics.rs` :grin: 

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:63 on 2023-01-12 17:50_

Much appreciated :)

---

_@charliermarsh reviewed on 2023-01-12 17:50_

---

_Comment by @charliermarsh on 2023-01-12 17:51_

> Please merge via rebase.

(I'll always merge your PRs via rebase if there are multiple commits.)


---

_Merged by @charliermarsh on 2023-01-12 18:09_

---

_Closed by @charliermarsh on 2023-01-12 18:10_

---

_Branch deleted on 2023-01-12 19:07_

---

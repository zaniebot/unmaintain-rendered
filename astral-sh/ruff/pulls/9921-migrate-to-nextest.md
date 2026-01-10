```yaml
number: 9921
title: "Migrate to `nextest`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - ci
assignees: []
merged: true
base: main
head: charlie/nextest
created_at: 2024-02-10T03:18:29Z
updated_at: 2024-02-12T14:47:17Z
url: https://github.com/astral-sh/ruff/pull/9921
synced_at: 2026-01-10T22:57:09Z
```

# Migrate to `nextest`

---

_Pull request opened by @charliermarsh on 2024-02-10 03:18_

## Summary

We've had success with `nextest` in other projects, so lets migrate Ruff.

The Linux tests look a little bit faster (from 2m32s down to 2m8s), the Windows tests look a little bit slower but not dramatically so.

---

_Review requested from @zanieb by @charliermarsh on 2024-02-10 03:33_

---

_Review requested from @konstin by @charliermarsh on 2024-02-10 03:33_

---

_Marked ready for review by @charliermarsh on 2024-02-10 03:35_

---

_Comment by @charliermarsh on 2024-02-10 03:35_

Should I recommend `nextest` in the contributing docs? It's not strictly required.

---

_Label `internal` added by @charliermarsh on 2024-02-10 03:38_

---

_Comment by @github-actions[bot] on 2024-02-10 03:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @konstin on `.github/workflows/ci.yaml`:151 on 2024-02-10 14:26_

`--all` is deprecated

```suggestion
        run: cargo nextest run --workspace --status-level skip --failure-output immediate-final --no-fail-fast -j 12
```

---

_Review comment by @konstin on `.github/workflows/ci.yaml`:123 on 2024-02-10 14:29_

Is `-j 12` correct for default size runners? The default runners have 2-4 cores (https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners#standard-github-hosted-runners-for-public-repositories), only the large runners have 12 (https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-larger-runners#about-macos-larger-runners). CC @zanieb iirc you benchmarked this?

---

_Review comment by @konstin on `.github/workflows/ci.yaml`:120 on 2024-02-10 14:31_

Are we fine with losing the `--unreferenced reject` here?

---

_Review comment by @konstin on `.github/workflows/ci.yaml`:123 on 2024-02-10 14:31_

`--all` is deprecated

```suggestion
        run: cargo nextest run --workspace --status-level skip --failure-output immediate-final --no-fail-fast -j 12
```

---

_Review comment by @konstin on `CONTRIBUTING.md`:87 on 2024-02-10 14:34_

We should add a note about `--failure-output immediate-final --no-fail-fast -j 12`

---

_@konstin approved on 2024-02-10 14:34_

---

_@charliermarsh reviewed on 2024-02-10 14:59_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:120 on 2024-02-10 14:59_

Yes

---

_@zanieb reviewed on 2024-02-10 16:19_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:123 on 2024-02-10 16:19_

Yeah I benchmarked this for our other project where I believe we have 8 cores. I found going above the number of cores was beneficial to a point (the default just uses the same number of jobs as you have cores).

I'd say 12 is probably too high over here with the small runner.

---

_@charliermarsh reviewed on 2024-02-10 18:23_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:87 on 2024-02-10 18:23_

Honestly tempted to omit `nextest` entirely since it just complicates things for new users.

---

_@charliermarsh reviewed on 2024-02-10 18:23_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:123 on 2024-02-10 18:23_

I'll try a few sizes.

---

_Comment by @charliermarsh on 2024-02-10 18:51_

Basically no difference:

| OS  | Workers | Time |
| ------------- | ------------- |  ------------- |
| Linux  | 4 | 2m11s 
| Linux  | 8 | 2m10s
| Linux  | 12 | 2m7s
| Windows  | 4 | 4m57s 
| Windows | 8 | 4m48s
| Windows  | 12 | 4m29s

---

_Merged by @charliermarsh on 2024-02-10 18:58_

---

_Closed by @charliermarsh on 2024-02-10 18:58_

---

_Branch deleted on 2024-02-10 18:58_

---

_Comment by @MichaReiser on 2024-02-12 08:38_

There are a few differences with the new setup compared to before

* No longer runs doctests
* Doesn't test all features or all targets
* Doesn't fail on unreferenced snapshots

---

_@MichaReiser reviewed on 2024-02-12 08:39_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:120 on 2024-02-12 08:39_

Hm, I would prefer keeping `--unreferenced reject`. It's useful to find old snapshots

---

_Label `ci` added by @MichaReiser on 2024-02-12 09:09_

---

_@MichaReiser reviewed on 2024-02-12 09:22_

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:87 on 2024-02-12 09:22_

I would omit it, especially the part about recommending it because nextest doesn't run doctests (needs `cargo test --doc`), and needs to be installed

---

_@charliermarsh reviewed on 2024-02-12 13:58_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:120 on 2024-02-12 13:58_

It’s not possible (IIUC) to preserve this with Nextest, and I just don’t think it’s the right tradeoff to choose the test runner based on this. It’s something we can run locally every once in a while.

---

_Comment by @charliermarsh on 2024-02-12 14:39_

I'll fix the `--all-features` thing, that was unintentional (we no longer use `--all-features` in other projects, and I copied this over).

---

_@charliermarsh reviewed on 2024-02-12 14:47_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:120 on 2024-02-12 14:47_

Oh, it looks like you _can_ use Nextest with Insta? I'm confused, I'd never looked into it but there was a conversation elsewhere suggesting that it wasn't possible to preserve this with Nextest \cc @zanieb.

---

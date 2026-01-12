```yaml
number: 358
title: "Add `--no-build`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: no-build
created_at: 2023-11-07T14:42:25Z
updated_at: 2023-11-08T15:05:16Z
url: https://github.com/astral-sh/uv/pull/358
synced_at: 2026-01-12T16:03:54Z
```

# Add `--no-build`

---

_@konstin_

By default, we will build source distributions for both resolving and installing, running arbitrary code. `--no-build` adds an option to ban this and only install from wheels, no source distributions or git builds allowed. We also don't fetch these and instead report immediately.

I've heard from users for whom this is a requirement, i'm implementing it now because it's helpful for testing.

I'm thinking about adding a shared `PuffinSharedArgs` struct so we don't have to repeat each option everywhere. 

---

_Review comment by @konstin on `crates/puffin-traits/src/lib.rs`:66 on 2023-11-07 14:43_

This feels slightly misplaced, but the alternative would be moving the client into the build context interface (which would be reasonable, but a large refactoring)

---

_@konstin reviewed on 2023-11-07 14:43_

---

_@charliermarsh reviewed on 2023-11-07 15:16_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:200 on 2023-11-07 15:16_

Contrary to the summary in the PR, I _think_ we will end up downloading sdists above but then failing here. Should we instead fail eagerly before `// Download any missing distributions.` by inspecting the remotes to see if any are source distributions?

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:33 on 2023-11-07 15:16_

We could also consider bitflags for these, since we'll have a number of flags here...

---

_@charliermarsh reviewed on 2023-11-07 15:16_

---

_@konstin reviewed on 2023-11-07 15:31_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:200 on 2023-11-07 15:31_

Shouldn't this check do this? https://github.com/astral-sh/puffin/pull/358/files#diff-5ba8a94412efc7027eb91366092458edf752eb50949307c67c4f8b7f52854037R79-R81

---

_@konstin reviewed on 2023-11-07 15:32_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:33 on 2023-11-07 15:32_

I'd just parse the whole args struct

---

_@charliermarsh reviewed on 2023-11-07 15:34_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:200 on 2023-11-07 15:34_

No, I don't think this uses that struct. That struct is only used in the resolver unfortunately. In the installer, we download the sdists in `Downloader`, then build them here in the builder.

---

_@konstin reviewed on 2023-11-08 11:08_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:200 on 2023-11-08 11:08_

Blocked downloads there too. I think downloads is the most reusable place to block for now (the alternative would be to block in the `DistributionFinder`)

---

_@charliermarsh approved on 2023-11-08 15:02_

---

_Merged by @charliermarsh on 2023-11-08 15:05_

---

_Closed by @charliermarsh on 2023-11-08 15:05_

---

_Branch deleted on 2023-11-08 15:05_

---

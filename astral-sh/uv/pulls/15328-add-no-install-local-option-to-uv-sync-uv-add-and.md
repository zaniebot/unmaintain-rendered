```yaml
number: 15328
title: "Add `--no-install-local` option to `uv sync`, `uv add` and `uv export` "
type: pull_request
state: merged
author: xaskii
labels: []
assignees: []
merged: true
base: main
head: xaskii/no-install-local
created_at: 2025-08-17T04:45:43Z
updated_at: 2025-08-22T20:59:57Z
url: https://github.com/astral-sh/uv/pull/15328
synced_at: 2026-01-12T16:11:41Z
```

# Add `--no-install-local` option to `uv sync`, `uv add` and `uv export` 

---

_@xaskii_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Closes #14866. Adds a `no-install-local` flag to the sync and export commands that excludes locally defined packages from being installed. 

This helps with if you're caching your virtual environment. You can exclude local packages since they're more likely to change between builds.

## Test Plan

snapshot test: `sync::no_install_local`
CI

## Notes
I made an `InstallOptions` struct to avoid a crate isolation issue I was running into while implementing. 

Thanks for maintaining this project!

---

_Marked ready for review by @xaskii on 2025-08-17 05:15_

---

_@zanieb reviewed on 2025-08-18 18:18_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:5202 on 2025-08-18 18:18_

Can you include a workspace member in this test case too?

---

_@zanieb reviewed on 2025-08-18 18:19_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:5219 on 2025-08-18 18:19_

Do we need to have an editable local package too since that's a different match type?

---

_Assigned to @zanieb by @zanieb on 2025-08-18 18:19_

---

_@xaskii reviewed on 2025-08-20 13:25_

---

_Review comment by @xaskii on `crates/uv/tests/it/sync.rs`:5202 on 2025-08-20 13:25_

Yep, just added one

---

_Review comment by @xaskii on `crates/uv/tests/it/sync.rs`:5219 on 2025-08-20 13:28_

Yeah that would make sense, just added one. Also, do you think I should change the `is_local` in this diff to a big match statement in case someone adds another entry to the Source enum: https://github.com/astral-sh/uv/pull/15328/files#diff-94a57a0e91f87a94e015da3bd8f9e8660ce3c8564718e78ee6599a3e214965d3R3699-R3706

---

_@xaskii reviewed on 2025-08-20 13:28_

---

_Comment by @xaskii on 2025-08-20 14:26_

Just rebased past #15375 (adding --no-install-* to `uv add`), and incorporated Charlie's changes

---

_Comment by @xaskii on 2025-08-20 14:28_

and updated docs

---

_Renamed from "Add `--no-install-local` option to `uv sync` and `uv export`" to "Add `--no-install-local` option to `uv sync`, `uv add` and `uv export` " by @xaskii on 2025-08-20 14:34_

---

_Review requested from @zanieb by @xaskii on 2025-08-20 17:39_

---

_@zanieb approved on 2025-08-22 16:31_

---

_Merged by @zanieb on 2025-08-22 16:31_

---

_Closed by @zanieb on 2025-08-22 16:31_

---

_Branch deleted on 2025-08-22 17:16_

---

_Comment by @danielgafni on 2025-08-22 20:59_

I think [this](https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs) section of the Docker tutorial should have this option instead of `--no-editable` now!  

---

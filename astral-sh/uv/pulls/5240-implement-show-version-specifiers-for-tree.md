```yaml
number: 5240
title: "Implement `--show-version-specifiers` for `tree`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: pip-tree-version-specifier
created_at: 2024-07-20T06:43:34Z
updated_at: 2024-07-20T19:04:45Z
url: https://github.com/astral-sh/uv/pull/5240
synced_at: 2026-01-12T16:06:42Z
```

# Implement `--show-version-specifiers` for `tree`

---

_@ChannyClaus_

## Summary
resolves https://github.com/astral-sh/uv/issues/5217

## Test Plan
existing tests pass (should be perfectly backwards compatible) + added a few tests to cover the new functionality. in particular, in addition to the simple use of `--emit-version-specifier`, its interaction with `--invert` and `--package` flags are tested.

---

_@ChannyClaus reviewed on 2024-07-20 07:08_

---

_Review comment by @ChannyClaus on `crates/uv/tests/pip_tree.rs`:1536 on 2024-07-20 07:08_

conflicting feelings towards the space after the comma - curious what others think

---

_@ChannyClaus reviewed on 2024-07-20 07:13_

---

_Review comment by @ChannyClaus on `crates/uv/tests/pip_tree.rs`:1692 on 2024-07-20 07:13_

```suggestion
        └── scikit-learn v1.4.1.post1 [requires: scipy >=1.6.0]
```
feeling similarly conflicted about the lack of space after the package name here

---

_@Enayaaa reviewed on 2024-07-20 07:23_

---

_Review comment by @Enayaaa on `crates/uv/tests/pip_tree.rs`:1692 on 2024-07-20 07:23_

Purely from feeling i think space should be emited :)

---

_Renamed from "implement `--emit-version-specifier`" to "implement `--emit-version-specifier` for `tree`" by @ChannyClaus on 2024-07-20 13:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-20 15:43_

---

_Label `enhancement` added by @charliermarsh on 2024-07-20 15:43_

---

_Label `cli` added by @charliermarsh on 2024-07-20 15:43_

---

_@zanieb reviewed on 2024-07-20 15:49_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2766 on 2024-07-20 15:49_

Can we add a short flag for this too?

We should probably use `--show-` not `--emit-` to match the rest of our CLI. `--emit-` is specific to the `pip compile` output.

---

_@charliermarsh reviewed on 2024-07-20 17:49_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2766 on 2024-07-20 17:49_

I think a short flag is hard here (`-e` and `-s` are both useful for global things (e.g., we use both in Ruff), it would be a shame to make them blocked on this flag in the future).

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2766 on 2024-07-20 17:53_

Yeah maybe "-V" or similar would make sense? I don't think a short-flag for the "emit" or "show" portion makes sense.

---

_@zanieb reviewed on 2024-07-20 17:53_

---

_@charliermarsh approved on 2024-07-20 18:23_

---

_@charliermarsh reviewed on 2024-07-20 18:24_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2766 on 2024-07-20 18:24_

Apparently `-V` is in use by `--version` already (fails at runtime), interesting.

---

_Renamed from "implement `--emit-version-specifier` for `tree`" to "Implement `--show-version-specifiers` for `tree`" by @charliermarsh on 2024-07-20 18:24_

---

_@charliermarsh reviewed on 2024-07-20 18:25_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/tree.rs`:329 on 2024-07-20 18:25_

I added a little more abstraction around this and the formatting code.

---

_Merged by @charliermarsh on 2024-07-20 18:31_

---

_Closed by @charliermarsh on 2024-07-20 18:31_

---

_@zanieb reviewed on 2024-07-20 19:04_

---

_Review comment by @zanieb on `crates/uv/tests/pip_tree.rs`:1536 on 2024-07-20 19:04_

This is the canonical display for version specifiers 🤷‍♀️ 

---

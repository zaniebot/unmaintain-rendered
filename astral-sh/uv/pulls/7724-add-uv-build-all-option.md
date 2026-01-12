```yaml
number: 7724
title: "Add `uv build --all` option"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: build-all
created_at: 2024-09-26T19:24:10Z
updated_at: 2024-09-27T07:58:33Z
url: https://github.com/astral-sh/uv/pull/7724
synced_at: 2026-01-12T16:07:57Z
```

# Add `uv build --all` option

---

_@blueraft_

## Summary

Resolves #7705 

## Test Plan

`cargo test` and tested locally.

The snapshots were unstable due to the packages being built in a non-deterministic order, so I used the quiet flag to suppress the output.

Another question is whether we should label the build output to indicate which package it belongs to?

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-26 19:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-26 19:34_

---

_Label `enhancement` added by @charliermarsh on 2024-09-26 19:34_

---

_Comment by @charliermarsh on 2024-09-26 19:36_

Wondering if we should call this `--workspace` to follow Cargo's convention? 

---

_Comment by @blueraft on 2024-09-26 19:44_

`--all` is a bit more intuitive to me, but `--workspace` sounds good too.

---

_@zanieb reviewed on 2024-09-26 19:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/build.rs`:286 on 2024-09-26 19:44_

You could also sort the results for determinism here right? Generally I'm pretty into having deterministic output if it's not high-cost.

---

_@charliermarsh reviewed on 2024-09-26 19:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build.rs`:286 on 2024-09-26 19:55_

I think the build output itself would be interleaved even if sorted. We'd need `--no-build-output` from your prior PR... then we could make the _asset names_ deterministic, I think.

---

_Comment by @charliermarsh on 2024-09-26 19:56_

I guess `--all` is fine since it's consistent with Rye.

---

_@charliermarsh reviewed on 2024-09-26 19:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build.rs`:153 on 2024-09-26 19:56_

Is `anyhow::Error` necessary? I think that's the default.

---

_Comment by @charliermarsh on 2024-09-26 19:57_

This generally looks good to me though it may be worth reviving #7675 so that we can get better snapshot tests (just the artifact output, rather than the build output). Wdyt @zanieb?

---

_@zanieb reviewed on 2024-09-26 23:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/build.rs`:286 on 2024-09-26 23:08_

We would need that.. wouldn't we :p I feel like in this case it'd probably make sense to do something else with the build output by default instead of interleaving it... like show progress bars for each build and write the output to a file per project or something. Anyway, that doesn't need to be in scope here.

Cross-linking for reference #7675 


---

_Comment by @zanieb on 2024-09-26 23:09_

I'd be down to restore https://github.com/astral-sh/uv/pull/7675 as a hidden option so we can use it here and consider if we want it in the long run?

---

_Comment by @charliermarsh on 2024-09-26 23:22_

Sounds good. I re-opened and approved.

---

_@charliermarsh reviewed on 2024-09-27 01:47_

---

_Review comment by @charliermarsh on `crates/uv/tests/build.rs`:1307 on 2024-09-27 01:47_

I think we should prefix these with the member name (like `[member_a] Building source distribution...`).

---

_@charliermarsh reviewed on 2024-09-27 01:47_

---

_Review comment by @charliermarsh on `crates/uv/tests/build.rs`:1330 on 2024-09-27 01:47_

Should every member write to a single top-level `./dist`? I'm actually not sure. What does Rye do? In this PR, you're writing to `./dist` relative to each member.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build.rs`:249 on 2024-09-27 01:50_

I think we need to omit virtual packages, and probably fail if there are no members to build.

---

_@charliermarsh reviewed on 2024-09-27 01:50_

---

_@charliermarsh approved on 2024-09-27 02:23_

---

_@charliermarsh reviewed on 2024-09-27 02:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build.rs`:249 on 2024-09-27 02:23_

Added this.

---

_@charliermarsh reviewed on 2024-09-27 02:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/build.rs`:1330 on 2024-09-27 02:23_

I guess this is fine. I'm not sure what's best. Rye writes to a single `./dist`.

---

_Merged by @charliermarsh on 2024-09-27 02:46_

---

_Closed by @charliermarsh on 2024-09-27 02:46_

---

_Comment by @charliermarsh on 2024-09-27 02:46_

Thanks!

---

_Branch deleted on 2024-09-27 07:58_

---

```yaml
number: 6895
title: "Implement `uv build`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/build
created_at: 2024-08-31T17:53:54Z
updated_at: 2024-09-04T15:23:49Z
url: https://github.com/astral-sh/uv/pull/6895
synced_at: 2026-01-12T16:07:34Z
```

# Implement `uv build`

---

_@charliermarsh_

## Summary

This PR exposes uv's PEP 517 implementation via a `uv build` frontend, such that you can use `uv build` to build source and binary distributions (i.e., wheels and sdists) from a given directory.

There are some TODOs that I'll tackle in separate PRs:

- [x] Support building a wheel from a source distribution (rather than from source) (#6898)
- [x] Stream the build output (#6912)

Closes https://github.com/astral-sh/uv/issues/1510

Closes https://github.com/astral-sh/uv/issues/1663.


---

_Label `enhancement` added by @charliermarsh on 2024-08-31 17:54_

---

_Label `cli` added by @charliermarsh on 2024-08-31 17:54_

---

_Marked ready for review by @charliermarsh on 2024-08-31 18:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-31 18:38_

---

_Review requested from @konstin by @charliermarsh on 2024-08-31 18:38_

---

_Comment by @charliermarsh on 2024-08-31 18:40_

I will do a pass on the docs once the interface gets the green light.

---

_Comment by @charliermarsh on 2024-09-01 16:10_

I plan to show build output in a later PR.

---

_Review comment by @konstin on `crates/uv/tests/build.rs`:2 on 2024-09-02 08:02_

This seems to have no effect on my machine

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:346 on 2024-09-02 08:03_

Where can I find these files after they are built?

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2935 on 2024-09-02 08:05_

The whole block seems to be dead code?

---

_Review comment by @konstin on `crates/uv/src/commands/build.rs`:63 on 2024-09-02 08:22_

Should we should the relative path here instead of the filename?

---

_Review comment by @konstin on `crates/uv/src/commands/build.rs`:253 on 2024-09-02 08:24_

We should explain this behavior in the cli docs

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:194 on 2024-09-02 08:38_

That's from a different PR

---

_Review comment by @konstin on `crates/uv/src/commands/build.rs`:231 on 2024-09-02 08:39_

We should make this configurable and add a `.gitignore` by default (similar to cache dirs)

---

_@konstin approved on 2024-09-02 08:40_

---

_Comment by @konstin on 2024-09-02 08:45_

Should `uv build` build the entire workspace or only the current package?

---

_@charliermarsh reviewed on 2024-09-03 18:41_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build.rs`:231 on 2024-09-03 18:41_

Will do RE `outdir`. Not sure on `.gitignore `though...

---

_Comment by @zanieb on 2024-09-03 18:53_

I would imagine building the entire workspace by default and `--package` to select a specific one?

---

_Comment by @charliermarsh on 2024-09-03 18:56_

I’d prefer to tackle any workspace integration in a separate PR. How would that intersect with the ability to pass a single source dist file? Or the ability to pass a path to a directory?

---

_@zanieb reviewed on 2024-09-03 18:58_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1954 on 2024-09-03 18:58_

```suggestion
    /// Build a binary distribution ("wheel") from the given directory.
```

---

_@zanieb reviewed on 2024-09-03 18:58_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1944 on 2024-09-03 18:58_

Should we just say "distributions"
```suggestion
    /// The directory from which distributions should be built.
```

---

_@zanieb reviewed on 2024-09-03 19:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/build.rs`:144 on 2024-09-03 19:00_

Can we support `--no-project` here? Trying to do that everywhere. Might not make sense for this command though?

---

_@zanieb reviewed on 2024-09-03 19:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/build.rs`:129 on 2024-09-03 19:00_

Should this be searching in the target directory instead of the working directory?

---

_Comment by @zanieb on 2024-09-03 19:05_

Another todo is to add documentation to

- The publishing guide
- The project concept

---

_Comment by @zanieb on 2024-09-03 19:06_

> How would that intersect with the ability to pass a single source dist file? Or the ability to pass a path to a directory?

I think if we're give a file target, we can just build that. If we're given a directory, we should treat that as the working directory for workspace / project discovery?

Related request at https://github.com/astral-sh/uv/issues/1510#issuecomment-2313700499 (not much more context there)

---

_Comment by @charliermarsh on 2024-09-03 23:41_

I will add `--outdir` separately.

---

_Comment by @charliermarsh on 2024-09-04 00:24_

Nevermind, I added `--out-dir` support here. I will do workspaces in another PR.

---

_Comment by @charliermarsh on 2024-09-04 01:33_

Ok, I added docs in https://github.com/astral-sh/uv/pull/6991, and workspace support (just `--package` for now) in #6990, so in theory the whole stack is ready to merge and ship once approved.

---

_Review requested from @zanieb by @charliermarsh on 2024-09-04 13:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/build.rs`:186 on 2024-09-04 14:29_

Fyi there is a `impl<'a> TryFrom<BaseClientBuilder<'a>> for RegistryClientBuilder<'a>` so we can avoid some repetition here? Kind of annoying it creates a temporary cache though.

---

_@zanieb reviewed on 2024-09-04 14:29_

---

_@zanieb approved on 2024-09-04 14:30_

---

_Comment by @charliermarsh on 2024-09-04 15:23_

Again `PermissionError: [Errno 13] Permission denied: 'E:\/uv-tmp\/[TMP]/__init__.py'`.

---

_Merged by @charliermarsh on 2024-09-04 15:23_

---

_Closed by @charliermarsh on 2024-09-04 15:23_

---

_Branch deleted on 2024-09-04 15:23_

---

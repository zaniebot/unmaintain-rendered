```yaml
number: 14003
title: When validating lockfile, take account of indexes defined as sources in path dependencies
type: pull_request
state: closed
author: jtfmumm
labels:
  - bug
assignees: []
base: main
head: jtfm/custom-indexes-lockfile
created_at: 2025-06-12T18:24:27Z
updated_at: 2025-07-25T05:06:50Z
url: https://github.com/astral-sh/uv/pull/14003
synced_at: 2026-01-10T06:53:01Z
```

# When validating lockfile, take account of indexes defined as sources in path dependencies

---

_Pull request opened by @jtfmumm on 2025-06-12 18:24_

As described in #11419, when an index was defined as a source in a path dependency (and that index ended up in the lockfile), it would cause lockfile validation to incorrectly determine that the file had changed. This PR adds those indexes to the list uv uses to validate. 

This adds those path dependency source indexes as a new field on `Workspace` (including from transitive path dependencies). It includes cycle detection to handle the case of circular path dependencies.

This includes test for explicit indexes in a path dependency, a combination of explicit and non-explicit, path dependencies without indexes defined, nested (transitive) path dependencies, and circular path dependencies. The circular path dependency test does not hang (indicating successful cycle detection) but lockfile validation still indicates the file has changed. This is not a change from `main` so I have marked this as a `TODO`.

Closes #11419

---

_Label `bug` added by @jtfmumm on 2025-06-12 18:24_

---

_@jtfmumm reviewed on 2025-06-12 18:26_

---

_Review comment by @jtfmumm on `crates/uv-workspace/src/workspace.rs`:1619 on 2025-06-12 18:26_

I've taken a permissive approach here since we are checking for `pyproject.toml` files defined in path dependencies, and only for the purposes of validation. That's why I've chosen to log problems canonicalizing and parsing them and then to continue.

---

_@jtfmumm reviewed on 2025-06-12 18:27_

---

_Review comment by @jtfmumm on `crates/uv-workspace/src/workspace.rs`:1653 on 2025-06-12 18:27_

See comment above

---

_@jtfmumm reviewed on 2025-06-12 18:28_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/lock.rs`:1086 on 2025-06-12 18:28_

I'm using a `Cow` here because the common case will probably be to have no path dependencies. This avoids cloning in that common case

---

_Marked ready for review by @jtfmumm on 2025-06-12 19:31_

---

_Assigned to @konstin by @zanieb on 2025-06-13 11:41_

---

_Review requested from @konstin by @zanieb on 2025-06-13 11:41_

---

_Review comment by @jtfmumm on `crates/uv-workspace/src/workspace.rs`:1610 on 2025-06-13 12:11_

I'm currently only handling the `Path` case from the issue. We could also fetch `Source::Git` and `Source::Url` `pyproject`s but I wasn't sure if we wanted to do that here.

---

_@jtfmumm reviewed on 2025-06-13 12:11_

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:27562 on 2025-06-17 10:53_

```suggestion
/// <https://github.com/astral-sh/uv/issues/11419>
```

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:1643 on 2025-06-17 12:01_

We should avoid reparsing `pyproject.toml`, this is an expensive operation when it runs as part of a noop `uv run`. See https://github.com/astral-sh/uv/pull/12096 for context and benchmarking instructions.

Can we reuse the parsed file through using the workspace cache around `collect_members_only`, or alternatively collect this information after workspace discovery so we can read from a warm cache of workspace members?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:1415 on 2025-06-17 12:58_

I can't comment down there, but can you add a comment near the `while let Some(package) = queue.pop_front() {` below that says something like:

> Unlike path dependencies, Git dependencies are immutable, their sources cannot change without the hashes changing, so we know their indexes are still present.

---

_@konstin reviewed on 2025-06-17 13:03_

---

_Comment by @charliermarsh on 2025-06-22 23:56_

Could we instead solve this by adding any explicit index assignments to the lockfile? We already write the normalized requirements (`requires-dev`), so this would just be an extension of that behavior. Taking the indexes into account in the way it's outlined here seems like an abstraction leak.

---

_Closed by @jtfmumm on 2025-07-25 05:06_

---

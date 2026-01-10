```yaml
number: 10682
title: Discard insufficient fork markers
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/discard-invalid-fork-markers
created_at: 2025-01-16T14:43:16Z
updated_at: 2025-03-13T14:49:39Z
url: https://github.com/astral-sh/uv/pull/10682
synced_at: 2026-01-10T11:10:34Z
```

# Discard insufficient fork markers

---

_Pull request opened by @konstin on 2025-01-16 14:43_

In #10669, a pyproject.toml with requires-python but no environment had a lockfile covering only a subset of the requires-python space:

```toml
resolution-markers = [
    "python_full_version >= '3.10' and platform_python_implementation == 'CPython'",
    "python_full_version == '3.9.*'",
    "python_full_version < '3.9'",
]
```

This marker set is invalid, we have to reject the lockfile. (We can still use the versions though, to avoid churn).

Part 1/2 of #10669

---

_Converted to draft by @konstin on 2025-01-16 14:43_

---

_Assigned to @konstin by @konstin on 2025-01-16 14:43_

---

_Unassigned @konstin by @konstin on 2025-01-16 14:43_

---

_Label `bug` added by @konstin on 2025-01-16 14:43_

---

_Comment by @charliermarsh on 2025-01-16 16:00_

Once discarded, do we just produce the same lockfile anyway? (If so, does this actually fix anything?)

---

_Comment by @konstin on 2025-01-16 16:07_

That's the other half of https://github.com/astral-sh/uv/issues/10669: the lockfile we're seeing is invalid, it doesn't cover all possible environments. We should never produce a lockfile in this state (fixing #10669), this new branch is error recovery from a buggy state.

This PR exists because i minimized https://github.com/astral-sh/uv/issues/10669 to this problem by keeping the lockfile, while we still need to fix the problem where we produce this lockfile starting from a pyproject.toml.

---

_Comment by @charliermarsh on 2025-01-16 16:11_

Ok. My question is: does this actually produce the correct lockfile? Or even with this change, are we still producing the incorrect lockfile after invalidating?

---

_Comment by @konstin on 2025-01-17 11:01_

If checked that when running `uv sync` on attrs' `uv-lock` branch, we warn and fix the markers in the lockfile.

There are two ways to create the lockfile:

A) 

```
git switch uv-lock
uv-this-pr sync
```

B)

```
git switch uv-lock
rm uv.lock
uv-this-pr sync
```

(A) has a slightly larger diff to `uv-lock` than (B) because we collapse e.g. zipp 3.20.2 and zip 3.21.0 into only zipp 3.20.2. I think this is acceptable for an error recovery branch.

---

_Marked ready for review by @konstin on 2025-01-17 11:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:906 on 2025-01-17 17:18_

Nit: "the user" (typo)

---

_@charliermarsh reviewed on 2025-01-17 17:18_

---

_@charliermarsh reviewed on 2025-01-17 17:19_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:910 on 2025-01-17 17:19_

supported environments, with an s? Not sure.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:908 on 2025-01-17 17:19_

Do they have to be exactly equal...? Could one be a superset?

---

_@charliermarsh reviewed on 2025-01-17 17:19_

---

_@konstin reviewed on 2025-01-17 18:15_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:908 on 2025-01-17 18:15_

I'm on the fence whether to do an equality check or a contains check, i like the equality check because it should catch more bad markers, but we can already revert that to `if fork_markers_union.negate().is_disjoint(environments_union)`.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:908 on 2025-01-17 18:54_

Lets do the more conservative thing (contains) for now... I'm just worried that this could have a significant impact and doesn't have a ton of testing, does it?

---

_@charliermarsh reviewed on 2025-01-17 18:54_

---

_Comment by @charliermarsh on 2025-01-17 19:45_

I'll tweak and merge this.

---

_@charliermarsh reviewed on 2025-01-17 20:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:908 on 2025-01-17 20:15_

I would prefer to change this but don't feel confident enough doing it myself, so it may miss this release.

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:908 on 2025-01-21 10:22_

done

---

_@konstin reviewed on 2025-01-21 10:22_

---

_@charliermarsh approved on 2025-03-11 02:54_

---

_Comment by @charliermarsh on 2025-03-11 02:54_

I'm a little nervous about this change but don't really have better ideas on how to validate it.

---

_Comment by @konstin on 2025-03-11 11:02_

What do think about this? We check that the fork marker coverage is correct for the entire test suite.

---

_Merged by @konstin on 2025-03-13 14:49_

---

_Closed by @konstin on 2025-03-13 14:49_

---

_Branch deleted on 2025-03-13 14:49_

---

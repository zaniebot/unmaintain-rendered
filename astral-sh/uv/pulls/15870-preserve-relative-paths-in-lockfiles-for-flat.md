```yaml
number: 15870
title: Preserve relative paths in lockfiles for flat indices
type: pull_request
state: open
author: harshithvh
labels:
  - bug
assignees: []
base: main
head: hvh/preserve-relative-paths
created_at: 2025-09-15T14:08:13Z
updated_at: 2025-11-11T14:39:44Z
url: https://github.com/astral-sh/uv/pull/15870
synced_at: 2026-01-12T16:11:59Z
```

# Preserve relative paths in lockfiles for flat indices

---

_@harshithvh_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Modified `Source::from_index_url` to check the original user input and preserve relative paths when the user originally provided them.
- Preserves user intent for relative paths
- Maintains backward compatibility for absolute paths

fixes: #15055

## Test Plan

added `lock_find_links_relative_path_preserved`, validates that flat indices preserve relative paths in lockfiles instead of converting them to absolute paths

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:11275 on 2025-09-15 14:11_

We can skip this check, we already see it in the snapshot above

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:3576 on 2025-09-15 14:15_

I would call `uv_fs::normalize_path` here so the lockfile uses a normalized path

---

_@konstin approved on 2025-09-15 14:16_

Thank you!

---

_Review requested from @charliermarsh by @konstin on 2025-09-15 14:16_

---

_@harshithvh reviewed on 2025-09-15 17:23_

---

_Review comment by @harshithvh on `crates/uv/tests/it/lock.rs`:11275 on 2025-09-15 17:23_

makes sense

---

_Comment by @harshithvh on 2025-09-16 07:03_

hey @konstin 
Can we close this if all looks good?
Thanks!

---

_Comment by @konstin on 2025-09-16 07:39_

I want to give @charliermarsh a chance to look at this too, afterwards we can merge it.

---

_Label `bug` added by @konstin on 2025-09-16 07:39_

---

_Comment by @charliermarsh on 2025-09-16 13:29_

I will review today.

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:11262 on 2025-09-16 14:03_

Sorry, I completely missed this: This is the part we actually want to have fixed, this is still capturing the absolute path. It seems that the change currently doesn't actually change the test case.

---

_@konstin reviewed on 2025-09-16 14:03_

---

_@charliermarsh reviewed on 2025-09-16 14:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:11267 on 2025-09-16 14:03_

Notice that we already get this right (even on main). We need to apply similar logic to `[package.metadata]`.

---

_@harshithvh reviewed on 2025-09-19 14:31_

---

_Review comment by @harshithvh on `crates/uv/tests/it/lock.rs`:11262 on 2025-09-19 14:31_

thanks for pointing this!

---

_Comment by @harshithvh on 2025-09-19 15:28_

@konstin, all looks good to me
mind giving a final review

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:11239 on 2025-09-25 08:59_

```suggestion
    let lock = fs_err::read_to_string(workspace.join("uv.lock"))?;
```

---

_Review comment by @konstin on `crates/uv-distribution-types/src/requirement.rs`:944 on 2025-09-25 08:59_

In which case does the code above fail but the logic below pass?

---

_Review comment by @konstin on `crates/uv-distribution-types/src/requirement.rs`:946 on 2025-09-25 09:01_

We must not unwrap in deserialization code: Even though uv would never write something invalid, someone or something may edit the lockfile to something invalid, and we need to report an error instead of panicking.

---

_@konstin reviewed on 2025-09-25 09:01_

---

_@harshithvh reviewed on 2025-10-31 10:30_

---

_Review comment by @harshithvh on `crates/uv-distribution-types/src/requirement.rs`:944 on 2025-10-31 10:30_

Yes, it’s redundant. `from_url_or_path` already accepts both valid URLs and paths; anything `DisplaySafeUrl::parse` would accept is already accepted by `from_url_or_path`.

---

_Comment by @harshithvh on 2025-10-31 10:43_

hey @konstin, sorry for keeping this PR hanging. Please review the changes when you have some time

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:3588 on 2025-11-06 17:03_

Can you add some documentation around why we need both the `relative_to` and reading the relative path from given?

---

_@konstin reviewed on 2025-11-06 17:03_

---

_@harshithvh reviewed on 2025-11-06 19:47_

---

_Review comment by @harshithvh on `crates/uv-resolver/src/lock/mod.rs`:3588 on 2025-11-06 19:47_

added docs! Hope this helps future readers

---

_@konstin reviewed on 2025-11-11 14:39_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:3588 on 2025-11-11 14:39_

Sorry I still don't follow. When they are on different drives (I assume you mean Windows drive letters, not Unix mounts?), how can the user config use a relative path?

Can you write a test case for this? The current test case passes with only the `requirement.rs` changes applied.

---

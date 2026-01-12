```yaml
number: 3630
title: Support lossless serialization for Git dependencies in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/git-lock-source
created_at: 2024-05-16T21:20:37Z
updated_at: 2024-05-17T21:23:41Z
url: https://github.com/astral-sh/uv/pull/3630
synced_at: 2026-01-12T16:05:45Z
```

# Support lossless serialization for Git dependencies in lockfile

---

_@charliermarsh_

## Summary

This PR adds lossless deserialization for `GitSourceDist` distributions in the lockfile. Specifically, we now properly preserve the requested revision, the subdirectory, and the precise Git commit SHA.

## Test Plan

`cargo test`


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-16 21:20_

---

_@charliermarsh reviewed on 2024-05-16 21:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:821 on 2024-05-16 21:21_

I feel like these need to be unified (part of what I was getting at in previous comments).

---

_@charliermarsh reviewed on 2024-05-16 21:22_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:821 on 2024-05-16 21:22_

Perhaps `GitReference` becomes internal to the crate, and that crate uses `GitSource` as its public interface? Not sure. It's useful (within the crate) to know that a ref can't be a commit, for example.

---

_@charliermarsh reviewed on 2024-05-16 21:22_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:625 on 2024-05-16 21:22_

I'm also storing the subdirectory in the query string.

---

_Marked ready for review by @charliermarsh on 2024-05-16 21:22_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:295 on 2024-05-16 21:31_

Not actually sure _what_ we should track here. The verbatim URL?

---

_@charliermarsh reviewed on 2024-05-16 21:31_

---

_Comment by @charliermarsh on 2024-05-17 02:44_

I would be fine merging as-is but I expect the abstractions to evolve a bit as I add similar logic for direct URLs.

---

_Label `preview` added by @charliermarsh on 2024-05-17 03:13_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:291 on 2024-05-17 11:31_

Is there any case where this should be `None`?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:295 on 2024-05-17 11:33_

Afaik the only real usages are `DistributionId` and `ResourceId`, which atm still assume `Url` input.

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:215 on 2024-05-17 11:35_

What's the motivation for also showing the user input here?

---

_@konstin approved on 2024-05-17 11:39_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:291 on 2024-05-17 13:55_

Yeah I think deserialization should probably fail if there's no precise revision. Same deal with `GitSha::from_str`. That should probably be parsed at deserialization time.

(I don't think that needs to be fixed in this PR, but it would be good to add a FIXME comment noting that the `None` cases should be pushed up to deserialization time.)

---

_@charliermarsh reviewed on 2024-05-17 13:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:215 on 2024-05-17 13:56_

I think at the very least we need to preserve this because it's supposed to be written out to `direct_url.json` (we don't do this today, incorrectly -- we write the hash always).

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:831 on 2024-05-17 14:04_

I think it should be an error if we don't have a precise commit right? Without the precise commit revision, the lock file cannot guarantee reproducibility.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:821 on 2024-05-17 14:06_

Can you say more about what unification means in this context? I think as long as we require that a specific revision appears in the lock file (and the sub-directory if it exists), then I think that's all we actually need.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:215 on 2024-05-17 14:07_

Another thought here is that tags/branches/whatever are mutable. So if we have `rev=3.7.0` but it doesn't actually match the precision revision, then installation could error or emit a warning. But I'm not 100% certain that's the right UX. (And I could see arguments saying that's actually the wrong UX.)

---

_@BurntSushi approved on 2024-05-17 14:08_

Happy to have this come in as-is, with maybe a comment or two noting FIXMEs. :)

---

_@charliermarsh reviewed on 2024-05-17 14:11_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:821 on 2024-05-17 14:11_

Mostly just commenting that `GitReference` and `GitSource` are _nearly_ identical, except that `GitReference` contains more variants that break down `GitSource::Rev` in a way that's purely based on analyzing the value.

---

_Comment by @charliermarsh on 2024-05-17 14:11_

I'll fix up the "require a SHA" part in this PR.

---

_Merged by @charliermarsh on 2024-05-17 21:23_

---

_Closed by @charliermarsh on 2024-05-17 21:23_

---

_Branch deleted on 2024-05-17 21:23_

---

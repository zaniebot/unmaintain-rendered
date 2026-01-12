```yaml
number: 3528
title: Always require hashes for wheels in lockfile
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/hash-req
created_at: 2024-05-11T22:32:07Z
updated_at: 2024-05-14T14:22:47Z
url: https://github.com/astral-sh/uv/pull/3528
synced_at: 2026-01-12T16:05:41Z
```

# Always require hashes for wheels in lockfile

---

_@charliermarsh_

## Summary

I think we do _always_ want hashes for wheels (since a wheel is a zip file -- we can always hash it). But hashes are optional for _source_ distributions: if we have a Git repo as the source, there's no way to compute a hash of it in the standards.

In general, if the user ends up needing to build from source, then we compare the hash of the source; if they use a pre-built wheel, then we compare the hash of the wheel.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-11 22:32_

---

_Renamed from "Always require hashes for wheels" to "Always require hashes for wheels in lockfile" by @charliermarsh on 2024-05-11 22:32_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1162 on 2024-05-13 12:22_

Hmmm the purpose of this test was to check a case where a hash is given _but should not be present_. And the purpose of the test above was to check a case where a hash is missing _but should be present_. i.e., They should both be checking error cases.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:178 on 2024-05-13 12:29_

So I basically agree with the end effect here, but I don't think this is actually internally consistent. The reason why I made it possible for hash checking in wheels to be optional is because this is what the data model currently supports:

https://github.com/astral-sh/uv/blob/eab2b832a65f7957ce073046f39131592fb566e2/crates/uv-resolver/src/lock.rs#L739-L745

The problem here, as far as I can tell, is that not all "built" distributions have hashes attached to them. So when they make it to the lock file data structure, there's no hash to include.

What this means is that this checking will reject lock files that can be generated right now.

With that said, I do agree that this change is, I think, what we want our _end state_ to be. I don't think I feel strongly about whether we do the stricter checking now, but I did kind of feel like the checking should be consistent with our serialization. If not, then perhaps a comment here explaining the inconsistency would be helpful.

---

_@BurntSushi reviewed on 2024-05-13 12:29_

---

_Comment by @charliermarsh on 2024-05-14 14:22_

I agree with what you wrote.

---

_Closed by @charliermarsh on 2024-05-14 14:22_

---

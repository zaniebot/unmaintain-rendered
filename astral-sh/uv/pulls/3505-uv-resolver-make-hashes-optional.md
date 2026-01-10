```yaml
number: 3505
title: "uv-resolver: make hashes optional"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/lock-optional-hash
created_at: 2024-05-10T13:31:38Z
updated_at: 2024-05-10T14:32:31Z
url: https://github.com/astral-sh/uv/pull/3505
synced_at: 2026-01-10T14:37:54Z
```

# uv-resolver: make hashes optional

---

_Pull request opened by @BurntSushi on 2024-05-10 13:31_

This only makes hashes optional for wheels/sdists that come from
registires or direct URLs. For wheels/sdists that come from other
sources, a hash should not be present.

For path dependencies, a hash should not be present because the state of
the path dependency is not intended to be tracked in the lock file. This
is consistent with how other tools deal with path dependencies, and if
it were otherwise, the hash would I believe need to be updated for every
change to the path dependency.

For git dependencies (source dists only), a hash should not be present
because the lock will contain the specific commit revision hash. This is
functionally equivalent to a hash, and so a hash is redundant.

As part of this change, we validate the presence or absence of a hash
based on the dependency source. We also add our first regression tests.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-10 13:33_

---

_@BurntSushi reviewed on 2024-05-10 13:35_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1162 on 2024-05-10 13:35_

I wrote this lock file just to provoke an error where a hash was found but wasn't expected.

But I'm not sure this is right. And has led me to wonder what a path dependency with a wheel should actually look like in the lock file. Notice, for example, the repetition in the `source` field above with the `url` in the `wheel` entry. The data model currently allows, for example, a path dependency to have multiple wheels.

(This issue isn't really related to this PR, but the lock file here is odd and I wanted to call it out.)

---

_Label `internal` added by @BurntSushi on 2024-05-10 13:36_

---

_@charliermarsh reviewed on 2024-05-10 13:37_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:524 on 2024-05-10 13:37_

There are multiple kinds of path dependencies: a path to an archive (`.tar.gz` or `.whl`), and a path to a source tree (i.e., a directory). For the former, we can require hashes, right?

---

_@charliermarsh approved on 2024-05-10 13:58_

Seems reasonable, we can figure out source archive vs. directory stuff in separate PRs.

---

_Comment by @charliermarsh on 2024-05-10 13:58_

But may want to adjust some of the comments around that.

---

_@BurntSushi reviewed on 2024-05-10 14:05_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:524 on 2024-05-10 14:05_

For reference, I've created https://github.com/astral-sh/uv/issues/3506 to track this issue.

---

_Comment by @BurntSushi on 2024-05-10 14:05_

> But may want to adjust some of the comments around that.

Aye, I've added a TODO comment in `requires_hash` noting that it isn't fully correct in the case of path dependencies.

---

_Merged by @BurntSushi on 2024-05-10 14:32_

---

_Closed by @BurntSushi on 2024-05-10 14:32_

---

_Branch deleted on 2024-05-10 14:32_

---

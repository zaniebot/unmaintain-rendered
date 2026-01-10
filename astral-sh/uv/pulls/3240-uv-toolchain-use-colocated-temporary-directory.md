```yaml
number: 3240
title: "uv-toolchain: use colocated temporary directory"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fetch-python-tmpdir
created_at: 2024-04-24T14:44:32Z
updated_at: 2024-04-24T14:56:04Z
url: https://github.com/astral-sh/uv/pull/3240
synced_at: 2026-01-10T14:37:54Z
```

# uv-toolchain: use colocated temporary directory

---

_Pull request opened by @BurntSushi on 2024-04-24 14:44_

Previously, this would use the "system" temporary directory.
Because we rename the resulting directory to its final destination,
and because renaming is implemented via hardlinking, and because
hardlinking doesn't work across mount points, and because /tmp is
commonly on a different mount point than where `uv` is checked out,
we "fix" this by putting the temporary directory somewhere close to
the final destination of the fetched artifact.

There are alternatives we might consider pursuing. For example,
if the `rename` fails, then we should probably do a recursive
directory copy. But this is a quick fix for now and it also
consistent with colocation of other temporary directories in `uv`.

The main downside of this change is that if a user does ^C while
`uv-dev fetch-python` is running, then there is no mechanism for
cleaning up temporary directories.


---

_Review requested from @zanieb by @BurntSushi on 2024-04-24 14:44_

---

_@konstin approved on 2024-04-24 14:46_

We're using that for the installation into the cache, too, i think that's what we want to do here

---

_@zanieb approved on 2024-04-24 14:47_

---

_Label `internal` added by @zanieb on 2024-04-24 14:47_

---

_Merged by @BurntSushi on 2024-04-24 14:56_

---

_Closed by @BurntSushi on 2024-04-24 14:56_

---

_Branch deleted on 2024-04-24 14:56_

---

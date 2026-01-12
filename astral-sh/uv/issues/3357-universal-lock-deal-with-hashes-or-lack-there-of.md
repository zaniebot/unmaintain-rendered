```yaml
number: 3357
title: "universal-lock: deal with hashes (or lack there of) for some types of distributions"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-03T16:32:17Z
updated_at: 2024-05-14T23:07:26Z
url: https://github.com/astral-sh/uv/issues/3357
synced_at: 2026-01-12T15:58:43Z
```

# universal-lock: deal with hashes (or lack there of) for some types of distributions

---

_@BurntSushi_

Currently, a `ResolvedDist` does not always have a hash. This is a problem for the lock file where we'd ideally want hashes for _at least_ direct URL dependencies. We currently also don't have hashes for Git or Path dependencies, but I think we probably don't want those. (Cargo doesn't use hashes for that case either.)

So I think we need to fix this:

https://github.com/astral-sh/uv/blob/e33ff95575e8165078799548070210947d1cfad0/crates/uv-resolver/src/lock.rs#L688-L695

And this:

https://github.com/astral-sh/uv/blob/e33ff95575e8165078799548070210947d1cfad0/crates/uv-resolver/src/lock.rs#L601-L607

And then probably update the data structures so that `hash` isn't required for the `git` and `path` sources.

---

_Label `preview` added by @BurntSushi on 2024-05-03 16:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-14 17:08_

---

_Closed by @charliermarsh on 2024-05-14 23:07_

---

_Closed by @charliermarsh on 2024-05-14 23:07_

---

```yaml
number: 11259
title: Use a flock to avoid concurrent initialization of project environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/synchronize-virtualenv
created_at: 2025-02-05T19:53:10Z
updated_at: 2025-02-05T21:00:41Z
url: https://github.com/astral-sh/uv/pull/11259
synced_at: 2026-01-12T16:09:45Z
```

# Use a flock to avoid concurrent initialization of project environments

---

_@charliermarsh_

## Summary

If you `uv run` from the same directory via multiple processes at the same time, some of them will fail as they'll see an "incomplete" virtual environment.

Closes https://github.com/astral-sh/uv/issues/11219.


---

_Label `bug` added by @charliermarsh on 2025-02-05 19:53_

---

_Review requested from @Gankra by @charliermarsh on 2025-02-05 19:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-05 19:53_

---

_Marked ready for review by @charliermarsh on 2025-02-05 19:53_

---

_@zanieb reviewed on 2025-02-05 19:56_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 19:56_

Isn't this naughty? Aren't you supposed to grab some uv-specific tmp dir location?

(I thought we removed these intentionally in the past, the only remaining reference is in the trampolines)

---

_@zanieb reviewed on 2025-02-05 19:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 19:57_

Oh and https://github.com/astral-sh/uv/blob/d281f49103a08a6b77b20009d69e481a19221347/crates/uv-python/src/environment.rs#L311

---

_@zanieb approved on 2025-02-05 19:58_

---

_@charliermarsh reviewed on 2025-02-05 20:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 20:14_

Yeah, we do this elsewhere. I guess we could create a directory within `env::temp_dir` called `uv`?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 20:15_

Honestly I kind of think we should do this everywhere rather than creating `.lock` files in virtualenvs, etc.

---

_@charliermarsh reviewed on 2025-02-05 20:15_

---

_Merged by @charliermarsh on 2025-02-05 20:19_

---

_Closed by @charliermarsh on 2025-02-05 20:19_

---

_Branch deleted on 2025-02-05 20:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 20:28_

I thought there was a very intentional reason we used co-located temporary files or the cache instead of `/tmp`

---

_@zanieb reviewed on 2025-02-05 20:28_

---

_@Gankra reviewed on 2025-02-05 20:48_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 20:48_

I don't know if it's relevant here but OS tempdirs are popular sources of Different Filesystem, and moving a file across filesystems does not benefit from the same atomicity guarantees (it's like `cp` instead of `mv`).

---

_@Gankra reviewed on 2025-02-05 20:49_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 20:49_

(This is broadly possible with basically any random Other Directory, hence "do atomic moves/renames only in the ~same directory" is often advisable)

---

_@zanieb reviewed on 2025-02-05 21:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:751 on 2025-02-05 21:00_

Yeah I think this is where we landed in Discord — co-location is critical for any atomic rename operations.

---

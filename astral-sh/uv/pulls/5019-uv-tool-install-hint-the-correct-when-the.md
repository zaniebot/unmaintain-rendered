```yaml
number: 5019
title: "`uv tool install` hint the correct when the executable is available"
type: pull_request
state: merged
author: blueraft
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: hint-correct-package
created_at: 2024-07-12T19:00:51Z
updated_at: 2024-07-15T17:38:23Z
url: https://github.com/astral-sh/uv/pull/5019
synced_at: 2026-01-10T13:42:52Z
```

# `uv tool install` hint the correct when the executable is available

---

_Pull request opened by @blueraft on 2024-07-12 19:00_

## Summary

Resolves #5018.

## Test Plan

`cargo test`

<img width="704" alt="Screenshot 2024-07-12 at 22 16 53" src="https://github.com/user-attachments/assets/d2d4d85b-d6c3-4b47-8f1a-bb07112d5931">


---

_@blueraft reviewed on 2024-07-12 19:04_

---

_Review comment by @blueraft on `crates/uv/tests/common/mod.rs`:491 on 2024-07-12 19:04_

This feels a bit weird, but `add_shared_args` adds the `exclude-newer` arg anyways so it had to be unset. I wasn't able to install `fastapi==0.111` otherwise.

---

_Assigned to @zanieb by @zanieb on 2024-07-12 19:08_

---

_@zanieb reviewed on 2024-07-12 19:42_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:491 on 2024-07-12 19:42_

Hm that's weird. We should fix that in general.

Fyi there's a method to clear an environment variable, which would be better practice here.

Regardless, we can't merge with this because the transitive dependency versions will change in the future and the snapshot will break — can you install a different version of FastAPI that's available relative to our default exclude newer?

---

_@zanieb reviewed on 2024-07-12 19:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:330 on 2024-07-12 19:43_

I think for user-facing copy we're using "executables"
```suggestion
                warn!("Failed to determine executables for packages in the environment: {err}");
```

Could maybe drop "the / this environment" entirely?

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:332 on 2024-07-12 19:46_

It'd be nice to special-case the singleton case because I think it will be by far the most common. e.g.

"However, the executable `fastapi` is available via dependency `fastapi-cli`. Did you mean `uv tool install fastapi-cli`?"

---

_@zanieb reviewed on 2024-07-12 19:46_

---

_Review comment by @blueraft on `crates/uv/src/commands/tool/install.rs`:332 on 2024-07-12 20:18_

Done

---

_@blueraft reviewed on 2024-07-12 20:19_

---

_@blueraft reviewed on 2024-07-12 20:23_

---

_Review comment by @blueraft on `crates/uv/tests/common/mod.rs`:491 on 2024-07-12 20:23_

> Fyi there's a method to clear an environment variable, which would be better practice here.

The `exclude-newer` is set as an arg in the `add_shared_args` function. I didn't see any method to clear an arg, which is why I ended up overriding with an env variable instead.

>  can you install a different version of FastAPI that's available relative to our default exclude newer?

Unfortunately the previous version`fastapi==0.110` doesn't include `fastapi-cli` as a dependency so it doesn't list any executables. Do you know of any other library that could be used here instead 

---

_@blueraft reviewed on 2024-07-13 09:45_

---

_Review comment by @blueraft on `crates/uv/tests/common/mod.rs`:491 on 2024-07-13 09:45_

I've reverted this change and switched to a static date in the test itself. What do you think of this approach?

---

_@zanieb reviewed on 2024-07-13 15:43_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:491 on 2024-07-13 15:43_

Ah okay we need the newer version. Hm. I don't have any good alternatives. A newer static date is okay for now.

> The exclude-newer is set as an arg in the add_shared_args function. I didn't see any method to clear an arg

Ah. Oops, I see.

---

_Label `error messages` added by @zanieb on 2024-07-15 16:47_

---

_Label `preview` added by @zanieb on 2024-07-15 16:47_

---

_Comment by @zanieb on 2024-07-15 16:47_

Heads up I made some small changes in https://github.com/astral-sh/uv/pull/5019/commits/908b0856b2fe14dcec483a29a0e62dbc15771675 — lmk if you have any questions or concerns.

---

_@zanieb approved on 2024-07-15 16:47_

---

_Merged by @zanieb on 2024-07-15 16:54_

---

_Closed by @zanieb on 2024-07-15 16:54_

---

_Branch deleted on 2024-07-15 17:38_

---

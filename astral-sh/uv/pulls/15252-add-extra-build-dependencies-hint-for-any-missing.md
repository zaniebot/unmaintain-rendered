```yaml
number: 15252
title: "Add `extra-build-dependencies` hint for any missing module on build failure"
type: pull_request
state: merged
author: blueraft
labels: []
assignees: []
merged: true
base: main
head: use-pipreqs-build-error-hints
created_at: 2025-08-13T16:00:39Z
updated_at: 2025-08-14T17:00:58Z
url: https://github.com/astral-sh/uv/pull/15252
synced_at: 2026-01-12T16:11:39Z
```

# Add `extra-build-dependencies` hint for any missing module on build failure

---

_@blueraft_

Alternative to https://github.com/astral-sh/uv/pull/15251.

As suggested in https://github.com/astral-sh/uv/issues/15118#issuecomment-3175735416 

## Test Plan

`cargo test`

---

_Comment by @zanieb on 2025-08-13 20:11_

Sorry I broke `main`!

---

_Comment by @zanieb on 2025-08-13 20:29_

I think you can test this with something like https://github.com/astral-sh/uv/blob/323aa8f3324a2faaf286ad3a6bb84113c2d69e52/crates/uv/tests/it/sync.rs#L1571-L1601

---

_@zanieb reviewed on 2025-08-13 20:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1416 on 2025-08-13 20:30_

I think we just want to use the package name throughout here?

---

_@blueraft reviewed on 2025-08-13 21:06_

---

_Review comment by @blueraft on `crates/uv/tests/it/sync.rs`:1416 on 2025-08-13 21:06_

Done

---

_@zanieb reviewed on 2025-08-13 21:43_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/error.rs`:217 on 2025-08-13 21:43_

This seems workable but is a little weird? I wonder if we can parse the filename instead? I'm not entirely sure what's best. @charliermarsh may have thoughts.

---

_@blueraft reviewed on 2025-08-14 10:58_

---

_Review comment by @blueraft on `crates/uv-build-frontend/src/error.rs`:217 on 2025-08-14 10:58_

I found this function https://github.com/astral-sh/uv/blob/c4e5984258c13204bd0aedd183a93cb16d6b754f/crates/uv-pep508/src/lib.rs#L487

---

_Comment by @charliermarsh on 2025-08-14 11:22_

I'm a little worried about the impact it will have on binary size, did you check a release build before and after?

---

_Comment by @blueraft on 2025-08-14 12:10_

>  did you check a release build before and after?

Not much of an increase, only 32kbs more

```sh
❯ du -sk uv_with_pipreqs
45240   uv_with_pipreqs
❯ du -sk uv_without_pepreqs
45208   uv_without_pepreqs
```

---

_@zanieb reviewed on 2025-08-14 13:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:4977 on 2025-08-14 13:06_

Fwiw this instruction isn't sufficient, because then they also need to install all of the other build dependencies of `anyio`. I'm hesitant to suggest it without that additional information (and broadly hesitant to suggest it at all).

---

_@zanieb reviewed on 2025-08-14 13:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:5048 on 2025-08-14 13:07_

Can we drop the quotes on the key when they're not needed?

---

_@zanieb reviewed on 2025-08-14 13:08_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/error.rs`:228 on 2025-08-14 13:08_

Maybe we should split this into a utility function to make it clear what we're doing. I'd also probably call the variable `package_name` instead of `normalized_version_id`

---

_@blueraft reviewed on 2025-08-14 14:50_

---

_Review comment by @blueraft on `crates/uv/tests/it/pip_install.rs`:4977 on 2025-08-14 14:50_

Yeah I thought about this, but this matches the current hint we give for people facing issues with `torch` so I figured we shouldn't remove that.

---

_@zanieb reviewed on 2025-08-14 14:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:4977 on 2025-08-14 14:51_

I think now that we have the extra build dependencies it's actually okay to remove that, but... I'm okay with considering that separately. Can you open a tracking issue?

---

_@blueraft reviewed on 2025-08-14 14:58_

---

_Review comment by @blueraft on `crates/uv/tests/it/pip_install.rs`:4977 on 2025-08-14 14:58_

https://github.com/astral-sh/uv/issues/15283

---

_Renamed from "Add pipreqs module-to-package mapping for build time ModuleNotFoundError hints" to "Add `extra-build-dependencies` hint for any missing module on build failure" by @zanieb on 2025-08-14 16:52_

---

_Merged by @zanieb on 2025-08-14 17:00_

---

_Closed by @zanieb on 2025-08-14 17:00_

---

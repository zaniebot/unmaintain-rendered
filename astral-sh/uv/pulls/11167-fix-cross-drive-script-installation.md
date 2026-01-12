```yaml
number: 11167
title: " Fix cross-drive script installation"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-data-move
created_at: 2025-02-02T16:08:48Z
updated_at: 2025-02-12T18:00:44Z
url: https://github.com/astral-sh/uv/pull/11167
synced_at: 2026-01-12T16:09:42Z
```

#  Fix cross-drive script installation

---

_@konstin_

Fallback to copy if renaming a script doesn't work, similar to the site packages installation.

Fixes #11163

**Test Plan**

Failing:
```
docker volume rm -f gh11163_usr_local_bin
docker run -v gh11163_usr_local_bin:/usr/local/bin -e UV_LINK_MODE=copy -v $(pwd):/io -it --rm ghcr.io/astral-sh/uv:0.5-python3.11-bookworm-slim uv pip install --system ruff
```

Passing:
```
cargo build --target x86_64-unknown-linux-musl
docker volume rm -f gh11163_usr_local_bin
docker run -v gh11163_usr_local_bin:/usr/local/bin -e UV_LINK_MODE=copy -v $(pwd):/io -it --rm ghcr.io/astral-sh/uv:0.5-python3.11-bookworm-slim /io/target/x86_64-unknown-linux-musl/debug/uv pip install --system ruff
```

---

_Label `bug` added by @konstin on 2025-02-02 16:08_

---

_@konstin reviewed on 2025-02-02 16:09_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-02 16:09_

:/

---

_@charliermarsh reviewed on 2025-02-04 00:22_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:832 on 2025-02-04 00:22_

Are these parts just moved? Could you consider doing that in a separate PR?

---

_@charliermarsh reviewed on 2025-02-04 00:22_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:825 on 2025-02-04 00:22_

Nit: Debug, Copy, Clone?

---

_@charliermarsh reviewed on 2025-02-04 00:23_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:847 on 2025-02-04 00:23_

Nit: can we put this in the `err` branch instead of early return?

---

_@charliermarsh reviewed on 2025-02-04 00:25_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:468 on 2025-02-04 00:25_

I think this change is incorrect... If we have to change the permissions, we _must_ copy the file. Check out the Git history on this.

---

_@charliermarsh reviewed on 2025-02-11 02:32_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-11 02:32_

Should we do `copy_with_retry_sync` here, in the inner loop?

Also, does `rename_with_retry_sync` retry if the error is due to different drives? Hopefully not?

---

_@charliermarsh approved on 2025-02-11 02:32_

---

_@konstin reviewed on 2025-02-11 10:51_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-11 10:51_

Do we have a `copy_with_retry_sync`?

This is mostly guess work, my idea was the security checkers would make the `rename_with_retry_sync loop fail and then the copy would succeed first try, but we can make this more defensive if we have a good way to.

---

_@charliermarsh reviewed on 2025-02-11 13:54_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-11 13:54_

I was suggesting we create it? I think the more important question here though is whether we can catch the "different drive" error, or whether we need this catch-all `Err(err)` branch. Do you know?

---

_@charliermarsh reviewed on 2025-02-11 13:55_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:851 on 2025-02-11 13:55_

Same question here: can we make this branch exclusive to "different drive"? Is there an error kind for it?

---

_@charliermarsh reviewed on 2025-02-12 03:04_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-12 03:04_

Ok, I looked into it myself, and `CrossesDevices` is unstable. Please add a TODO here to catch that error when it stabilizes. Then decide whether you want to add a `copy_with_retry_sync` method (I would suggest doing so, but it's up to you) and go ahead with the merge.

---

_@konstin reviewed on 2025-02-12 17:59_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-12 17:59_

Ah now I get what you were getting at.

I now implemented the more defensive version where we retry in both rename or copy. The callback version is kinda ugly but it's only used in this one place. 

I didn't realize there was a dedicated error code for cross drive, i had initially assumed that this is part of some other platform-specific error code. Once it stabilized we can upgrade this code path.

---

_@konstin reviewed on 2025-02-12 17:59_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/wheel.rs`:504 on 2025-02-12 17:59_

If you have suggestions for a prettier version of `with_retry_sync` i'll follow up.

---

_Merged by @konstin on 2025-02-12 18:00_

---

_Closed by @konstin on 2025-02-12 18:00_

---

_Branch deleted on 2025-02-12 18:00_

---

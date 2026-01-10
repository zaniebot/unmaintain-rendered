```yaml
number: 16046
title: "Add `UV_SKIP_WHEEL_FILENAME_CHECK` to allow installing invalid wheels"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/legacy
created_at: 2025-09-28T00:08:30Z
updated_at: 2025-09-29T23:54:28Z
url: https://github.com/astral-sh/uv/pull/16046
synced_at: 2026-01-10T06:36:15Z
```

# Add `UV_SKIP_WHEEL_FILENAME_CHECK` to allow installing invalid wheels

---

_Pull request opened by @charliermarsh on 2025-09-28 00:08_

## Summary

This PR adds a user setting to allow (in rare cases) accepting wheels with mismatched filenames and internal metadata.

Closes https://github.com/astral-sh/uv/issues/8082.

Closes https://github.com/astral-sh/uv/issues/15647.


---

_Marked ready for review by @charliermarsh on 2025-09-28 00:08_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-28 00:08_

---

_Review request for @zanieb removed by @charliermarsh on 2025-09-28 00:08_

---

_Review requested from @konstin by @charliermarsh on 2025-09-28 00:08_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-28 00:08_

---

_Label `compatibility` added by @charliermarsh on 2025-09-28 00:08_

---

_@charliermarsh reviewed on 2025-09-28 00:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:3251 on 2025-09-28 00:10_

I would prefer to do this once and pass it around, but it's legitimately annoying to do here, because this is ultimately triggered by a `TryFrom` on `LockWire`, so even if we did have it as a setting, we'd have to restructure the Serde setup to make it work.

Honestly, I kind of think we should parse these once (`EnvironmentOptions`) then set some global flags on a singleton that we can then access anywhere.

---

_@charliermarsh reviewed on 2025-09-28 00:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:3251 on 2025-09-28 00:50_

I tried this out here: https://github.com/astral-sh/uv/pull/16047

---

_Review comment by @konstin on `crates/uv-install-wheel/src/lib.rs`:78 on 2025-09-29 08:31_

nit: It's definitely malformed

```suggestion
    #[error("Wheel package name does not match filename ({0} != {1}), which indicates a malformed wheel. If this is intentional, set `{env_var}`.", env_var = "UV_SKIP_WHEEL_FILENAME_CHECK=1".green())]
```

---

_@konstin approved on 2025-09-29 09:01_

---

_Merged by @charliermarsh on 2025-09-29 23:54_

---

_Closed by @charliermarsh on 2025-09-29 23:54_

---

_Branch deleted on 2025-09-29 23:54_

---

```yaml
number: 16284
title: Move parsing http retries to EnvironmentOptions
type: pull_request
state: merged
author: andrey-berenda
labels:
  - internal
assignees: []
merged: true
base: main
head: move-http-retries-to-EnvironmentOptions
created_at: 2025-10-13T15:17:02Z
updated_at: 2025-10-21T09:14:37Z
url: https://github.com/astral-sh/uv/pull/16284
synced_at: 2026-01-12T16:12:12Z
```

# Move parsing http retries to EnvironmentOptions

---

_@andrey-berenda_

## Summary
- Move  parsing `UV_HTTP_RETRIES` to `EnvironmentOptions`

Relates https://github.com/astral-sh/uv/issues/14720

## Test Plan

- Tests with existing tests

---

_Review comment by @andrey-berenda on `crates/uv-client/src/base_client.rs`:287 on 2025-10-13 15:17_

I made this function public and use this
Just not to use DEFAULT_RETRIES

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:639 on 2025-10-13 15:23_

I used 3 here
It should be the same as DEFAULT_RETRIES
I used 3 instead of DEFAULT_RETRIES because I do not want to add uv-client as deps here

---

_@andrey-berenda reviewed on 2025-10-13 15:29_

---

_Marked ready for review by @andrey-berenda on 2025-10-13 17:03_

---

_@konstin reviewed on 2025-10-20 11:40_

---

_Review comment by @konstin on `crates/uv-settings/src/lib.rs`:639 on 2025-10-20 11:40_

There is already a transitive dependency from uv-settings to uv-client, so adding the dependency is effectively for free.

<img width="1758" height="1691" alt="image" src="https://github.com/user-attachments/assets/1790d3a2-9c81-4089-913e-fb6ecf914f09" />


---

_Label `internal` added by @konstin on 2025-10-20 11:43_

---

_Review comment by @konstin on `crates/uv-settings/src/lib.rs`:758 on 2025-10-20 11:48_

Can we show the actual error message from parsing instead of our generic one? This solves the error message regression.

After doing that, we shouldn't need those wrapper functions anymore, but can use e.g. `parse_integer_environment_variable::<u32>` directly.

---

_@konstin reviewed on 2025-10-20 11:51_

---

_@konstin reviewed on 2025-10-20 11:52_

---

_Review comment by @konstin on `crates/uv/tests/it/network.rs`:354 on 2025-10-20 11:52_

This is the regression in the error message I mentioned in the other comment.

---

_@aberenda-optifino reviewed on 2025-10-20 12:13_

---

_Review comment by @aberenda-optifino on `crates/uv-settings/src/lib.rs`:758 on 2025-10-20 12:13_

Fixed

---

_Review comment by @aberenda-optifino on `crates/uv/tests/it/network.rs`:354 on 2025-10-20 12:13_

Fixed

---

_@aberenda-optifino reviewed on 2025-10-20 12:13_

---

_@konstin approved on 2025-10-20 12:17_

Thank you!

---

_Comment by @aberenda-optifino on 2025-10-20 13:02_

Could you please retry this?
https://github.com/astral-sh/uv/actions/runs/18651784234/job/53171692215?pr=16284
It seems only this is required to merge
@konstin  

---

_Comment by @konstin on 2025-10-20 14:41_

Our depot runner are affected by the AWS outage (https://status.depot.dev/), sorry for the disruption.

---

_Merged by @konstin on 2025-10-21 09:14_

---

_Closed by @konstin on 2025-10-21 09:14_

---

```yaml
number: 2891
title: Fix linehaul tests
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/fix-linehaul-test
created_at: 2024-04-08T04:23:07Z
updated_at: 2024-04-11T00:08:54Z
url: https://github.com/astral-sh/uv/pull/2891
synced_at: 2026-01-10T14:43:31Z
```

# Fix linehaul tests

---

_Pull request opened by @zanieb on 2024-04-08 04:23_

Cleans up the assertions a bit. I looked into snapshot tests per #2564 but it didn't seem worth it for cross-platform tests.

Closes #2564 
Closes https://github.com/astral-sh/uv/pull/2878

---

_Label `internal` added by @zanieb on 2024-04-08 04:23_

---

_Merged by @zanieb on 2024-04-08 04:42_

---

_Closed by @zanieb on 2024-04-08 04:42_

---

_Branch deleted on 2024-04-08 04:42_

---

_Review comment by @konstin on `crates/uv-client/tests/user_agent_version.rs`:202 on 2024-04-10 13:34_

This breaks running the tests locally

---

_@konstin reviewed on 2024-04-10 13:34_

---

_@zanieb reviewed on 2024-04-10 13:46_

---

_Review comment by @zanieb on `crates/uv-client/tests/user_agent_version.rs`:202 on 2024-04-10 13:46_

Really? This should be pulled from the platform information we're passing directly right?

---

_@konstin reviewed on 2024-04-10 14:09_

---

_Review comment by @konstin on `crates/uv-client/tests/user_agent_version.rs`:202 on 2024-04-10 14:09_

For linux we're not passing the information in we're using the actual platform (`sys_info::linux_os_release()`)

```
thread 'test_user_agent_has_linehaul' panicked at crates/uv-client/tests/user_agent_version.rs:200:9:
assertion `left == right` failed
  left: Some("mantic")
 right: Some("jammy")
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

(mantic is what i'm running locally)

---

_@zanieb reviewed on 2024-04-10 14:11_

---

_Review comment by @zanieb on `crates/uv-client/tests/user_agent_version.rs`:202 on 2024-04-10 14:11_

Ah I see I'm sorry we should hardcode that.

---

_@zanieb reviewed on 2024-04-10 14:14_

---

_Review comment by @zanieb on `crates/uv-client/tests/user_agent_version.rs`:202 on 2024-04-10 14:14_

I see we can't pass mock data in... uhh hmm.

---

_@samypr100 reviewed on 2024-04-11 00:08_

---

_Review comment by @samypr100 on `crates/uv-client/tests/user_agent_version.rs`:202 on 2024-04-11 00:08_

This should hopefully be fixed in https://github.com/astral-sh/uv/pull/2923 since it uses `sys_info::linux_os_release()` now, which is what the actual linehaul code uses.

---

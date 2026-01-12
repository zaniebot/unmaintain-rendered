```yaml
number: 14544
title: "Add `UV_HTTP_RETRIES` to customize retry counts"
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
assignees: []
merged: true
base: main
head: zb/retries
created_at: 2025-07-10T15:54:07Z
updated_at: 2025-07-11T12:35:29Z
url: https://github.com/astral-sh/uv/pull/14544
synced_at: 2026-01-12T16:11:16Z
```

# Add `UV_HTTP_RETRIES` to customize retry counts

---

_@zanieb_

I want to increase this number in CI and was surprised we didn't support configuration yet.


---

_Label `configuration` added by @zanieb on 2025-07-10 15:54_

---

_@charliermarsh reviewed on 2025-07-10 16:00_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:137 on 2025-07-10 16:00_

Should this be a fatal error?

---

_@zanieb reviewed on 2025-07-10 16:39_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:137 on 2025-07-10 16:39_

Plausibly yeah, but I was hesitant to change the type in the PoC

---

_@zanieb reviewed on 2025-07-10 17:12_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:137 on 2025-07-10 17:12_

Pretty messy to do so, but such is the state of constructing HTTP clients in uv.

---

_@zanieb reviewed on 2025-07-10 17:14_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:264 on 2025-07-10 17:14_

We need this so a test that we consume all the retries isn't slow.

---

_@zanieb reviewed on 2025-07-10 17:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:199 on 2025-07-10 17:15_

I gave up here, it looked annoying and we only need the client for fetching seed packages.

---

_@zanieb reviewed on 2025-07-10 17:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:199 on 2025-07-10 17:27_

Removing miette in #14546 

---

_Marked ready for review by @zanieb on 2025-07-10 18:15_

---

_Renamed from "Add `UV_HTTP_RETRIES`" to "Add `UV_HTTP_RETRIES` to customize retry counts" by @zanieb on 2025-07-10 18:15_

---

_@charliermarsh approved on 2025-07-10 23:08_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:179 on 2025-07-10 23:08_

Can probably be `to_str` since if it's non-UTF-8 it won't parse, right?

---

_@charliermarsh reviewed on 2025-07-10 23:08_

---

_@zanieb reviewed on 2025-07-10 23:19_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:179 on 2025-07-10 23:19_

Then I need to write an error case for when that's returns `None`, it seemed easier to just let it hit the parse error.

---

_Merged by @zanieb on 2025-07-11 12:35_

---

_Closed by @zanieb on 2025-07-11 12:35_

---

_Branch deleted on 2025-07-11 12:35_

---

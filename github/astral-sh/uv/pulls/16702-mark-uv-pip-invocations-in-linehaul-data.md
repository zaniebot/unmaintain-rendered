---
number: 16702
title: "Mark `uv pip` invocations in linehaul data"
type: pull_request
state: closed
author: zsol
labels:
  - registry
  - uv pip
assignees: []
draft: true
base: main
head: zsol/jj-yspoqwnxoqkt
created_at: 2025-11-12T11:13:48Z
updated_at: 2025-12-05T10:54:06Z
url: https://github.com/astral-sh/uv/pull/16702
synced_at: 2026-01-07T13:12:19-06:00
---

# Mark `uv pip` invocations in linehaul data

---

_Pull request opened by @zsol on 2025-11-12 11:13_

## Summary

This PR makes uv set the `installer.name` field in the linehaul metadata to `uv-pip` for all registry requests made from `uv pip` commands.

## Test Plan

- [x] Added a test case to capture that setting `installer_name` on a `ClientBuilder` results in the user agent header changed accordingly.
- [x] Ran a few manual tests and observed linehaul data being sent and parsed:
```
UV_DEFAULT_INDEX=https://api.pyx.dev/simple/public/pypi cargo run -- pip install httpx torch
```


---

_Renamed from "Mark uv pip invocations in linehaul data" to "Mark `uv pip` invocations in linehaul data" by @zsol on 2025-11-12 11:31_

---

_Label `registry` added by @zsol on 2025-11-12 11:32_

---

_Label `uv pip` added by @zsol on 2025-11-12 11:32_

---

_Review requested from @charliermarsh by @zsol on 2025-11-12 12:05_

---

_Review requested from @zanieb by @zsol on 2025-11-12 12:05_

---

_Review requested from @konstin by @zsol on 2025-11-12 12:05_

---

_Marked ready for review by @zsol on 2025-11-12 12:05_

---

_Review comment by @konstin on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-12 12:55_

Slightly surprised but linehaul doesn't seem to check this field at all, which is good for us.

---

_@konstin approved on 2025-11-12 12:59_

---

_@samypr100 reviewed on 2025-11-13 05:48_

---

_Review comment by @samypr100 on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 05:48_

~As part of Google BigQuery? I feel I've queried based on installer name in the past 😅 e.g. `details.installer.name`~

Ah, you meant the cloud function doesn't validate the field contents. Ignore my initial reaction.

---

_@samypr100 reviewed on 2025-11-13 05:53_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/user_agent_version.rs`:25 on 2025-11-13 05:53_

Note, a new abstraction to spawing this server will be in https://github.com/astral-sh/uv/pull/16473

---

_@samypr100 reviewed on 2025-11-13 06:02_

---

_Review comment by @samypr100 on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 06:02_

I'd expect changing details.installer.name could have some consequences / churn in terms of how statistics are reported for uv in cases where someone using Big Query doesn't group by details.installer.name and does a select on `details.installer.name = 'uv'` directly for popular clients. I'd expect this to be a common scenario to avoid associated costs.

---

_@konstin reviewed on 2025-11-13 09:47_

---

_Review comment by @konstin on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 09:47_

Ideally we had an additional field so that querying for uv can be done in a single condition instead of splitting this into two names.

---

_Review comment by @samypr100 on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 17:31_

Maybe @pradyunsg, @di or @ewdurbin may have better ideas on how to avoid splitting `details.installer.name` between `uv` and `uv-pip` or this use case in general?

I think that'd be great if adding a new field could be a supported option to avoid the confusion of having two potential installer names, but I'm not sure what type of coordination or process would be needed in linehaul's side first. There may be better options I'm missing.

---

_@samypr100 reviewed on 2025-11-13 17:31_

---

_@zanieb reviewed on 2025-11-13 18:35_

---

_Review comment by @zanieb on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 18:35_

We also posted in the PyPA Discord https://discord.com/channels/803025117553754132/885686683507494913/1438509254964281435

---

_@zanieb reviewed on 2025-11-13 18:35_

---

_Review comment by @zanieb on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 18:35_

I think we won't merge this as-is.

---

_Converted to draft by @zanieb on 2025-11-13 18:35_

---

_@ewdurbin reviewed on 2025-11-13 18:47_

---

_Review comment by @ewdurbin on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-13 18:47_

Seems entirely reasonable to add additional values under the details key, though that can take some time since we no longer get to run our own schema migrations now that our dataset is part of the google public datasets. file an issue at https://github.com/pypi/linehaul-cloud-function/issues and tag me

---

_Closed by @zsol on 2025-11-13 19:43_

---

_@zsol reviewed on 2025-11-18 10:26_

---

_Review comment by @zsol on `crates/uv-client/src/linehaul.rs`:129 on 2025-11-18 10:26_

ref: https://github.com/pypi/linehaul-cloud-function/issues/233

---

_Referenced in [astral-sh/uv#16837](../../astral-sh/uv/pulls/16837.md) on 2025-11-24 17:09_

---

_Branch deleted on 2025-12-05 10:54_

---

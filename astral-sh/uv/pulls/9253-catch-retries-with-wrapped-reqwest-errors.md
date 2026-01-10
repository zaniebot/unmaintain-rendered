```yaml
number: 9253
title: "Catch retries with wrapped `reqwest` errors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-11-19T21:56:11Z
updated_at: 2024-11-19T22:08:49Z
url: https://github.com/astral-sh/uv/pull/9253
synced_at: 2026-01-10T12:00:00Z
```

# Catch retries with wrapped `reqwest` errors

---

_Pull request opened by @charliermarsh on 2024-11-19 21:56_

## Summary

It turns out that `WrappedReqwestError` skips the `reqwest::Error` itself in order to hack the display. This PR adds it to the list of variants we check when retrying transient errors.

Closes https://github.com/astral-sh/uv/issues/9246.


## Test Plan

Patched `reqwest` locally to return an error in `bytes()`. Verified that it was _not_ caught prior to this PR, but was caught afterwards.


---

_Label `bug` added by @charliermarsh on 2024-11-19 21:56_

---

_@charliermarsh reviewed on 2024-11-19 21:56_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:465 on 2024-11-19 21:56_

We could also change `WrappedReqwestError`, it's a bit of a footgun, but I guess it's meant to _replace_ `reqwest::Error` rather than wrap it further.

---

_Review requested from @zanieb by @charliermarsh on 2024-11-19 21:58_

---

_Review requested from @konstin by @charliermarsh on 2024-11-19 21:58_

---

_@zanieb reviewed on 2024-11-19 22:00_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:465 on 2024-11-19 22:00_

Yeah... idk. Both approaches seem to have downsides. I'd sort of rather fix the footgun but I'm not sure what other implications that has, presumably it affects the display in a problematic way?

---

_@zanieb approved on 2024-11-19 22:00_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:465 on 2024-11-19 22:07_

`WrappedReqwestError` is a hack, if we have a better i'm all for it

---

_@konstin approved on 2024-11-19 22:08_

---

_@charliermarsh reviewed on 2024-11-19 22:08_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:465 on 2024-11-19 22:08_

Yeah it looked sorta tricky. I can try though. I'll merge this for now.

---

_Merged by @charliermarsh on 2024-11-19 22:08_

---

_Closed by @charliermarsh on 2024-11-19 22:08_

---

_Branch deleted on 2024-11-19 22:08_

---

```yaml
number: 13228
title: Improve trace log for retryable errors
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/retry-err
created_at: 2025-04-30T13:08:42Z
updated_at: 2025-05-04T12:25:30Z
url: https://github.com/astral-sh/uv/pull/13228
synced_at: 2026-01-10T11:10:41Z
```

# Improve trace log for retryable errors

---

_Pull request opened by @zanieb on 2025-04-30 13:08_

Previously, this looked like

> TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(405), url: "https://pkgs.dev.azure.com/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl" }))) }


---

_Label `tracing` added by @zanieb on 2025-04-30 13:08_

---

_Comment by @zanieb on 2025-04-30 13:13_

I think since this is in `is_extended_transient_error` we'll actually never retry these errors, so we could exit early there? but don't need to make that change here

---

_Closed by @zanieb on 2025-04-30 13:25_

---

_Reopened by @zanieb on 2025-04-30 13:25_

---

_Review requested from @charliermarsh by @zanieb on 2025-04-30 16:05_

---

_@BurntSushi approved on 2025-04-30 17:24_

---

_Merged by @zanieb on 2025-04-30 17:29_

---

_Closed by @zanieb on 2025-04-30 17:29_

---

_Branch deleted on 2025-04-30 17:29_

---

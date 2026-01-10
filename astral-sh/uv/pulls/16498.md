```yaml
number: 16498
title: "Limit `uv auth login pyx.dev` retries to 60s"
type: pull_request
state: merged
author: woodruffw
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ww/limit-login-retries
created_at: 2025-10-29T15:23:45Z
updated_at: 2025-10-29T15:39:25Z
url: https://github.com/astral-sh/uv/pull/16498
synced_at: 2026-01-10T06:36:16Z
```

# Limit `uv auth login pyx.dev` retries to 60s

---

_Pull request opened by @woodruffw on 2025-10-29 15:23_

## Summary

Without this, a user who does `uv auth login ...` will retry against the service's status endpoint forever. This probably isn't what they intended (they probably walked away from their machine), so we end their login initiation session after 60 retries. Since we do a retry every second, this gives them no less than a minute to complete a login (which should be more than enough).

## Test Plan

We don't have browser-negotiated login tests at the moment in CI, but I've tested this locally:

```console
% ./target/debug/uv auth login pyx.dev
Logging in with https://api.pyx.dev/auth/cli/login/REDACTED
error: Login session timed out
```

(That took well over a minute, so 60s is a lower bound assuming a very optimal network roundtrip on each poll.)

---

_Review requested from @zanieb by @woodruffw on 2025-10-29 15:23_

---

_Assigned to @woodruffw by @woodruffw on 2025-10-29 15:23_

---

_@zanieb approved on 2025-10-29 15:24_

---

_Comment by @woodruffw on 2025-10-29 15:27_

With explicit timeout:

```
 ./target/debug/uv auth login pyx.dev
Logging in with https://api.pyx.dev/auth/cli/login/REDACTED
error: Login session timed out after 60 seconds
```

---

_Renamed from "auth/login: limit the number of status retries" to "Limit the number of `uv login login` retries to 60s" by @konstin on 2025-10-29 15:31_

---

_Label `enhancement` added by @konstin on 2025-10-29 15:31_

---

_Comment by @woodruffw on 2025-10-29 15:33_

I'll fix that clippy issue in another PR.

---

_Comment by @konstin on 2025-10-29 15:35_

Clippy is a required check, we require it for merging. If you push another commit with it, the auto-merge will go through.

(I've update the PR title for the changelog)

---

_Renamed from "Limit the number of `uv login login` retries to 60s" to "Limit `uv auth login pyx.dev` retries to 60s" by @zanieb on 2025-10-29 15:39_

---

_Merged by @zanieb on 2025-10-29 15:39_

---

_Closed by @zanieb on 2025-10-29 15:39_

---

_Branch deleted on 2025-10-29 15:39_

---

```yaml
number: 16172
title: Upgrade reqsign to 0.18.0 to remove chrono deps
type: pull_request
state: merged
author: tisonkun
labels:
  - internal
assignees: []
merged: true
base: main
head: reqsign-018
created_at: 2025-10-08T02:38:39Z
updated_at: 2025-10-08T14:49:39Z
url: https://github.com/astral-sh/uv/pull/16172
synced_at: 2026-01-10T06:36:15Z
```

# Upgrade reqsign to 0.18.0 to remove chrono deps

---

_Pull request opened by @tisonkun on 2025-10-08 02:38_

This follows up https://github.com/astral-sh/uv/pull/15925.

cc @BurntSushi @charliermarsh

---

_Comment by @charliermarsh on 2025-10-08 02:41_

Woahh amazing, thank you!

---

_Comment by @tisonkun on 2025-10-08 05:11_

> Error: missing API token, please run `depot login`

Seems CI failure is unrelated to this patch but my own environment doesn't have the token.

Could you give a review? I think this is a relatively tiny one :D

---

_Renamed from "Upgrade resign to 0.18.0 to remove chrono deps" to "Upgrade reqsign to 0.18.0 to remove chrono deps" by @konstin on 2025-10-08 06:09_

---

_Label `internal` added by @konstin on 2025-10-08 06:16_

---

_Comment by @konstin on 2025-10-08 06:17_

> Seems CI failure is unrelated to this patch but my own environment doesn't have the token.

It's a problem with a CI provider, it affects all CI runs atm.

---

_Comment by @tisonkun on 2025-10-08 11:25_

@konstin, shall we merge this patch since the failure is unrelated? Or wait for some other conditions?

---

_Review requested from @charliermarsh by @konstin on 2025-10-08 13:10_

---

_Comment by @charliermarsh on 2025-10-08 13:47_

I'll test the upgrade locally, then merge.

---

_@charliermarsh approved on 2025-10-08 14:29_

Tested locally, LGTM.

---

_Merged by @charliermarsh on 2025-10-08 14:31_

---

_Closed by @charliermarsh on 2025-10-08 14:31_

---

_Branch deleted on 2025-10-08 14:49_

---

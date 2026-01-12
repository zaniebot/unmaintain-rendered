```yaml
number: 14480
title: Move fragment preservation test to directly test our redirect handling logic
type: pull_request
state: merged
author: jtfmumm
labels:
  - testing
assignees: []
merged: true
base: main
head: jtfm/remove-fragment-redirect
created_at: 2025-07-07T08:23:52Z
updated_at: 2025-07-07T10:51:23Z
url: https://github.com/astral-sh/uv/pull/14480
synced_at: 2026-01-12T16:11:14Z
```

# Move fragment preservation test to directly test our redirect handling logic

---

_@jtfmumm_

When [updating](https://github.com/astral-sh/uv/pull/14475) to the latest `reqwest` version, our fragment propagation test broke. That test was partially testing the `reqwest` behavior, so this PR moves the fragment test to directly test our logic for constructing redirect requests.

---

_Review requested from @konstin by @jtfmumm on 2025-07-07 08:23_

---

_@konstin approved on 2025-07-07 08:25_

Can you leave a note why we can ignore https://datatracker.ietf.org/doc/html/rfc7231#page-84 ?

---

_Renamed from "Remove fragment propagation in redirect requests" to "Move fragment preservation test to directly test our redirect handling logic" by @jtfmumm on 2025-07-07 09:16_

---

_Comment by @jtfmumm on 2025-07-07 09:25_

> Can you leave a note why we can ignore https://datatracker.ietf.org/doc/html/rfc7231#page-84 ?

It's possible that we're seeing a regression in the `reqwest` behavior. I'll investigate further.

---

_Label `testing` added by @jtfmumm on 2025-07-07 09:27_

---

_Label `do-not-merge` added by @jtfmumm on 2025-07-07 09:27_

---

_Label `do-not-merge` removed by @jtfmumm on 2025-07-07 09:29_

---

_Merged by @jtfmumm on 2025-07-07 10:51_

---

_Closed by @jtfmumm on 2025-07-07 10:51_

---

_Branch deleted on 2025-07-07 10:51_

---

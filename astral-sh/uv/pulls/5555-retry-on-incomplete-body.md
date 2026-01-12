```yaml
number: 5555
title: Retry on incomplete body
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - network
assignees: []
merged: true
base: main
head: konsti/also-retry-unexpected-eof
created_at: 2024-07-29T11:33:24Z
updated_at: 2024-07-29T13:53:25Z
url: https://github.com/astral-sh/uv/pull/5555
synced_at: 2026-01-12T16:06:53Z
```

# Retry on incomplete body

---

_@konstin_

This is an attempt to add https://github.com/astral-sh/uv/issues/3514#issuecomment-2253562096 to retrying.

Relevant hyper code:
* https://github.com/hyperium/hyper/blob/15cd6fa1fc52f88b58283a673c134a97bea275d3/src/proto/h1/decode.rs#L683
* https://github.com/hyperium/hyper/blob/15cd6fa1fc52f88b58283a673c134a97bea275d3/src/proto/h1/decode.rs#L161-L164


---

_Label `enhancement` added by @konstin on 2024-07-29 11:33_

---

_Label `network` added by @konstin on 2024-07-29 11:33_

---

_@charliermarsh approved on 2024-07-29 13:42_

---

_Merged by @konstin on 2024-07-29 13:53_

---

_Closed by @konstin on 2024-07-29 13:53_

---

_Branch deleted on 2024-07-29 13:53_

---

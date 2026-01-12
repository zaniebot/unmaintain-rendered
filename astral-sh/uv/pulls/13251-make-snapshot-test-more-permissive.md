```yaml
number: 13251
title: make snapshot test more permissive
type: pull_request
state: closed
author: Gankra
labels:
  - internal
assignees: []
base: main
head: gankra/loose-test
created_at: 2025-05-01T12:12:03Z
updated_at: 2025-06-02T18:18:11Z
url: https://github.com/astral-sh/uv/pull/13251
synced_at: 2026-01-12T16:10:37Z
```

# make snapshot test more permissive

---

_@Gankra_

fixes #13212 

---

_Label `internal` added by @Gankra on 2025-05-01 12:12_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:946 on 2025-05-01 14:16_

Given this test won't really change a lot, should we just check if commit info is available and have two snapshots? Using filters this way makes me a bit uncomfortable.

---

_@zanieb reviewed on 2025-05-01 14:16_

---

_@Gankra reviewed on 2025-05-01 14:47_

---

_Review comment by @Gankra on `crates/uv/tests/it/version.rs`:946 on 2025-05-01 14:47_

oh interesting, i never considered conditional snapshots! that's a great idea

---

_@zanieb reviewed on 2025-05-01 17:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:946 on 2025-05-01 17:34_

They're just annoying during snapshot updates, so I try to avoid them in most cases.

---

_@zanieb reviewed on 2025-05-01 17:35_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:946 on 2025-05-01 17:35_

but if the filter is replacing basically the entire expected output, I'd either just skip the test or have a conditional.

---

_Comment by @mgorny on 2025-05-16 03:31_

Gentle ping. Can I do anything to help move this or any other solution forward?

---

_@mgorny reviewed on 2025-05-21 07:44_

---

_Review comment by @mgorny on `crates/uv/tests/it/version.rs`:946 on 2025-05-21 07:44_

I've filed #13566 doing that.

---

_Closed by @Gankra on 2025-06-02 18:18_

---

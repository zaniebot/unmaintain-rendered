```yaml
number: 16737
title: "Account for `python_downloads_json_url` on Pre-release Python version warnings"
type: pull_request
state: merged
author: MeitarR
labels: []
assignees: []
merged: true
base: main
head: fix-16711
created_at: 2025-11-14T11:57:13Z
updated_at: 2025-11-14T21:12:35Z
url: https://github.com/astral-sh/uv/pull/16737
synced_at: 2026-01-12T16:12:25Z
```

# Account for `python_downloads_json_url` on Pre-release Python version warnings

---

_@MeitarR_

Solves #16711

---

_Marked ready for review by @MeitarR on 2025-11-14 12:04_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1690 on 2025-11-14 16:47_

Where are we using this?

---

_@zanieb reviewed on 2025-11-14 16:47_

---

_@zanieb reviewed on 2025-11-14 16:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1690 on 2025-11-14 16:47_

i.e., when do we pass `Some` in here?

---

_@MeitarR reviewed on 2025-11-14 17:00_

---

_Review comment by @MeitarR on `crates/uv/tests/it/common/mod.rs`:1690 on 2025-11-14 17:00_

I think nowhere.

I'm ok with reverting this change

---

_@zanieb reviewed on 2025-11-14 18:14_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1690 on 2025-11-14 18:14_

Okay let's revert it then? 

---

_@MeitarR reviewed on 2025-11-14 18:39_

---

_Review comment by @MeitarR on `crates/uv/tests/it/common/mod.rs`:1690 on 2025-11-14 18:39_

done

---

_Review requested from @zanieb by @MeitarR on 2025-11-14 19:53_

---

_Merged by @zanieb on 2025-11-14 21:12_

---

_Closed by @zanieb on 2025-11-14 21:12_

---

```yaml
number: 12941
title: Add test for install from remote requirements.txt
type: pull_request
state: merged
author: jtfmumm
labels:
  - testing
assignees: []
merged: true
base: main
head: jtfm/remote-reqs-test
created_at: 2025-04-17T12:42:42Z
updated_at: 2025-04-17T14:25:40Z
url: https://github.com/astral-sh/uv/pull/12941
synced_at: 2026-01-12T16:10:27Z
```

# Add test for install from remote requirements.txt

---

_@jtfmumm_

Closes #2593.


---

_Review requested from @zanieb by @jtfmumm on 2025-04-17 12:42_

---

_@charliermarsh approved on 2025-04-17 13:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:561 on 2025-04-17 13:12_

Should this use a package that _isn't_ installed above, like `iniconfig`?

---

_@charliermarsh reviewed on 2025-04-17 13:12_

---

_@jtfmumm reviewed on 2025-04-17 13:16_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_install.rs`:561 on 2025-04-17 13:16_

Yeah it could. I was just imitating the existing `requirements.txt` test. I'll update both

---

_Label `testing` added by @jtfmumm on 2025-04-17 13:18_

---

_@zanieb reviewed on 2025-04-17 13:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:580 on 2025-04-17 13:49_

I might put this in `it/common/mod.rs` but don't feel strongly

---

_@zanieb approved on 2025-04-17 13:50_

---

_Closed by @jtfmumm on 2025-04-17 13:51_

---

_Reopened by @jtfmumm on 2025-04-17 13:51_

---

_@jtfmumm reviewed on 2025-04-17 14:25_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_install.rs`:580 on 2025-04-17 14:25_

I think some of my open PRs are the first time we've used mock servers outside of `middleware.rs`. Let me think about how to approach this and maybe I'll open a separate PR once these open ones are merged.

---

_Merged by @jtfmumm on 2025-04-17 14:25_

---

_Closed by @jtfmumm on 2025-04-17 14:25_

---

_Branch deleted on 2025-04-17 14:25_

---

```yaml
number: 5203
title: Process completed Python installs and uninstalls as a stream
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/collect
created_at: 2024-07-19T01:05:20Z
updated_at: 2024-07-19T12:50:39Z
url: https://github.com/astral-sh/uv/pull/5203
synced_at: 2026-01-12T16:06:41Z
```

# Process completed Python installs and uninstalls as a stream

---

_@charliermarsh_

## Summary

This ensures that we process Python installs and uninstalls as soon as they complete, rather than waiting for them all to complete, then processing them sequentially. In practice, it shouldn't be much of a difference (since the processing is code is fairly light), but it strikes me as more correct.


---

_Marked ready for review by @charliermarsh on 2024-07-19 01:22_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 01:22_

---

_Label `enhancement` added by @charliermarsh on 2024-07-19 01:22_

---

_Label `preview` added by @charliermarsh on 2024-07-19 01:22_

---

_@zanieb approved on 2024-07-19 01:38_

---

_@j178 reviewed on 2024-07-19 02:41_

---

_Review comment by @j178 on `crates/uv/src/commands/python/install.rs`:146 on 2024-07-19 02:41_

Is it possible that the progress bar might be interleaved with the error message in line 178?

---

_@j178 reviewed on 2024-07-19 04:00_

---

_Review comment by @j178 on `crates/uv/src/commands/python/install.rs`:146 on 2024-07-19 04:00_

Yes, it is. I manually altered one installation's SHA-256 checksum, and the progress bar was indeed interleaved by the hash mismatch error.
![image](https://github.com/user-attachments/assets/b69578a4-54c4-4d0a-9582-bd04f1e28fd9)


---

_@charliermarsh reviewed on 2024-07-19 11:36_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:146 on 2024-07-19 11:36_

Yeah I considered that at the time. Is it actually bad though? Otherwise you don’t get feedback until all downloads complete.

---

_@konstin reviewed on 2024-07-19 12:15_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:146 on 2024-07-19 12:15_

In my experience, if you don't clear the progress bar before printing (using the process bar's printer), the terminal likes to eat part of the error message

---

_@charliermarsh reviewed on 2024-07-19 12:28_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:146 on 2024-07-19 12:28_

Ok, I'll move it after then.

---

_Merged by @charliermarsh on 2024-07-19 12:50_

---

_Closed by @charliermarsh on 2024-07-19 12:50_

---

_Branch deleted on 2024-07-19 12:50_

---

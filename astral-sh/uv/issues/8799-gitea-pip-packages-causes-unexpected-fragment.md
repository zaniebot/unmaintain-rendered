---
number: 8799
title: "gitea pip packages causes unexpected fragment (expected `#sha256=...` or similar) sha256-xxxx"
type: issue
state: closed
author: athuyaoo
labels:
  - compatibility
assignees: []
created_at: 2024-11-04T08:04:06Z
updated_at: 2024-11-08T04:28:51Z
url: https://github.com/astral-sh/uv/issues/8799
synced_at: 2026-01-10T01:24:32Z
---

# gitea pip packages causes unexpected fragment (expected `#sha256=...` or similar) sha256-xxxx

---

_Issue opened by @athuyaoo on 2024-11-04 08:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
![image](https://github.com/user-attachments/assets/0037293a-e4ba-4013-a8a1-6c124305554d)


---

_Label `compatibility` added by @charliermarsh on 2024-11-04 13:45_

---

_Comment by @charliermarsh on 2024-11-04 13:47_

That looks like a spec violation: https://peps.python.org/pep-0503/. Where are these coming from? Do you have control over the format?

---

_Comment by @athuyaoo on 2024-11-08 04:28_

Thanks! Let me take a look at the format. It's from @go-gitea. https://github.com/go-gitea/gitea

---

_Closed by @athuyaoo on 2024-11-08 04:28_

---

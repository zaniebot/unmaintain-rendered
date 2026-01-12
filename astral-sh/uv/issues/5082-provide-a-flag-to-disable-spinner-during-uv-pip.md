```yaml
number: 5082
title: Provide a flag to disable spinner during uv pip sync/install
type: issue
state: closed
author: sambhav
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-07-15T21:25:37Z
updated_at: 2024-07-16T21:48:58Z
url: https://github.com/astral-sh/uv/issues/5082
synced_at: 2026-01-12T15:58:53Z
```

# Provide a flag to disable spinner during uv pip sync/install

---

_@sambhav_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv currently renders a spinner that might make it difficult to render logs in browser/web components easily as it can spew a lot of output. It would be great if uv could expose a flag similar to pip to disable the progress bar/spinner.


---

_Renamed from "Provide a flag to disable spinner" to "Provide a flag to disable spinner during uv pip sync/install" by @sambhav on 2024-07-15 21:25_

---

_Comment by @charliermarsh on 2024-07-16 01:17_

Yeah we should support `--no-progress` or something.

---

_Label `enhancement` added by @charliermarsh on 2024-07-16 01:17_

---

_Label `cli` added by @charliermarsh on 2024-07-16 01:17_

---

_Comment by @zanieb on 2024-07-16 04:17_

I felt like we had a discussion about this when adding the progress bars but can't find anything. 

---

_Comment by @silvanocerza on 2024-07-16 08:52_

A global `--no-progress-bar` option is a good idea I think. 

I can take care of this, though I don't know all the commands that print progress bars. 

---

_Comment by @charliermarsh on 2024-07-16 12:06_

IIRC we decided that we wanted to add it, but punted on it at the time :)

---

_Closed by @zanieb on 2024-07-16 21:48_

---

_Closed by @zanieb on 2024-07-16 21:48_

---

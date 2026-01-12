```yaml
number: 4379
title: UV editable install fails on databricks shared cluster 
type: issue
state: closed
author: ion-elgreco
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-06-18T09:42:08Z
updated_at: 2024-08-25T16:07:05Z
url: https://github.com/astral-sh/uv/issues/4379
synced_at: 2026-01-12T15:58:49Z
```

# UV editable install fails on databricks shared cluster 

---

_@ion-elgreco_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Try to run: `%sh uv pip install -e .`

Results in :

```
%sh uv pip install -e .
error: Failed to download and build: `my_package @ [file:///Workspace/Repos/<user>/<repo>`](file:///Workspace/Repos/<user>/<repo>%60)
  Caused by: Failed to build: `my_package @ [file:///Workspace/Repos/<user>/<repo>`](file:///Workspace/Repos/<user>/<repo>%60)
  Caused by: Build backend failed to determine extra requires with `build_editable()` with exit status: 1
--- stdout:
running egg_info
writing my_package .egg-info/PKG-INFO
--- stderr:
error: [Errno 1] Operation not permitted: '/Workspace/Repos/<user>/<repo>/my_package .egg-info/tmptjepatrp'
---
```

As simple `pip install -e .` without UV works.


---

_Comment by @zanieb on 2024-06-18 14:46_

Do you have write permissions to the directory?

Can you share verbose logs for `pip` and `uv pip`?

---

_Label `question` added by @zanieb on 2024-06-19 00:14_

---

_Label `needs-mre` added by @charliermarsh on 2024-06-23 16:03_

---

_Comment by @ion-elgreco on 2024-08-25 12:11_

@zanieb sorry for late response, got distracted with other stuff at work at that time. I don't have access to databricks anymore so can't provide an MRE at this time

---

_Comment by @charliermarsh on 2024-08-25 16:07_

No worries! Going to close for now since it will be hard for us to make progress on this.

---

_Closed by @charliermarsh on 2024-08-25 16:07_

---

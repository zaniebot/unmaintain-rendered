```yaml
number: 1740
title: Failed to copy databricks-0.2.dist-info/RECORD
type: issue
state: closed
author: ion-elgreco
labels: []
assignees: []
created_at: 2024-02-20T08:51:12Z
updated_at: 2024-02-20T15:41:06Z
url: https://github.com/astral-sh/uv/issues/1740
synced_at: 2026-01-10T05:40:31Z
```

# Failed to copy databricks-0.2.dist-info/RECORD

---

_Issue opened by @ion-elgreco on 2024-02-20 08:51_

It fails installing databricks since it's getting permission denied to copy the RECORD file? Install with pip works fine though

To reproduce:

```
uv venv
uv pip install databricks
```

```
error: Failed to install: databricks-0.2-py2.py3-none-any.whl (databricks==0.2)
  Caused by: failed to copy file from /home/ion/.cache/uv/archive-v0/sHSR-5YnmTL4pBq1yKiee/databricks-0.2.dist-info/RECORD to /home/ion/idp-simba/.venv/lib/python3.10/site-packages/databricks-0.2.dist-info/RECORD
  Caused by: Permission denied (os error 13)
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Assigned to @konstin by @konstin on 2024-02-20 09:05_

---

_Closed by @konstin on 2024-02-20 15:41_

---

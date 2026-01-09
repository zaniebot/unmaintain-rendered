---
number: 9141
title: "Feature request: add --no-deps support for TOML head dependencies section"
type: issue
state: closed
author: shelper
labels:
  - enhancement
assignees: []
created_at: 2024-11-15T04:42:27Z
updated_at: 2024-11-16T04:03:40Z
url: https://github.com/astral-sh/uv/issues/9141
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature request: add --no-deps support for TOML head dependencies section

---

_Issue opened by @shelper on 2024-11-15 04:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am using uv 0.5.2, 
i am trying to run a script with head of something like:
```
# /// script
# requires-python = ">=3.8,<3.9"
# dependencies = [
# "onnx",
# "onnxruntime",
# "numpy",
# "openvino2tensorflow==1.34.0",
# "tensorflow-datasets==4.9.1",
# "protobuf==3.20.3",
# "openvino==2021.4.2 --no-deps",
# ]
# ///
```

what i am looking for is a way to  override uv to install certain packages with option --no-deps so it ignores the conflicts as needed. 




---

_Comment by @charliermarsh on 2024-11-16 03:09_

We might need to support overrides and constraints in PEP 723 scripts. 

---

_Label `enhancement` added by @charliermarsh on 2024-11-16 03:09_

---

_Label `help wanted` added by @charliermarsh on 2024-11-16 03:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-16 03:46_

---

_Label `help wanted` removed by @charliermarsh on 2024-11-16 03:46_

---

_Referenced in [astral-sh/uv#9162](../../astral-sh/uv/pulls/9162.md) on 2024-11-16 03:50_

---

_Closed by @charliermarsh on 2024-11-16 04:03_

---

_Closed by @charliermarsh on 2024-11-16 04:03_

---

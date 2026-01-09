---
number: 6989
title: "Feature Request: generate the conda environment yaml based on uv.lock"
type: issue
state: open
author: shuowpro
labels:
  - wish
assignees: []
created_at: 2024-09-04T01:04:43Z
updated_at: 2024-09-04T13:04:20Z
url: https://github.com/astral-sh/uv/issues/6989
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature Request: generate the conda environment yaml based on uv.lock

---

_Issue opened by @shuowpro on 2024-09-04 01:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Based on this issue: #6007  we have already support export the requirement.txt file based on uv.lock. is it possible to support export the conda environment yaml file as well?


---

_Label `wish` added by @zanieb on 2024-09-04 02:57_

---

_Comment by @zanieb on 2024-09-04 02:57_

It's probably possible. Can you share more about your use-case?

---

_Comment by @shuowpro on 2024-09-04 03:22_

I have a remote server environment where I am required to use Anaconda for training the model. I want to manage and align the Anaconda environment with our build environment, and I think uv is a perfect tool for this. I'm curious about the long-term plan for uv in terms of supporting Anaconda. Exporting the Anaconda environment file would be sufficient for our needs. If you think this design aligns with your long-term plan, I am happy to help with its implementation.

---

_Comment by @zanieb on 2024-09-04 13:04_

I think it's okay to support if it works, but I'm not sure how well it will translate since Conda has, for example, different names for some packages. We can't actually perform a resolution with Conda.

---

_Referenced in [astral-sh/uv#1703](../../astral-sh/uv/issues/1703.md) on 2024-09-10 13:38_

---

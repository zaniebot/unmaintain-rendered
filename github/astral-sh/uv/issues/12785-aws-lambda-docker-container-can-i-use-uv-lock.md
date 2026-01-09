---
number: 12785
title: "AWS Lambda Docker Container -> can I use uv.lock?"
type: issue
state: closed
author: kurt-rhee
labels:
  - question
assignees: []
created_at: 2025-04-09T16:16:02Z
updated_at: 2025-04-09T20:00:09Z
url: https://github.com/astral-sh/uv/issues/12785
synced_at: 2026-01-07T13:12:18-06:00
---

# AWS Lambda Docker Container -> can I use uv.lock?

---

_Issue opened by @kurt-rhee on 2025-04-09 16:16_

### Question

I was wondering why install from requirements.txt instead of using uv.lock?

https://docs.astral.sh/uv/guides/integration/aws-lambda/#deploying-a-docker-image

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @kurt-rhee on 2025-04-09 16:16_

---

_Comment by @charliermarsh on 2025-04-09 16:17_

Because Lambda has some non-standard requirements, specifically, we have to export all your dependencies into a single folder (without a Python interpreter) and zip it up. So we need to use the `--target` option on `uv pip install`. (Note that this still installs from a `uv.lock`, because if you look at the docs, we use `uv export` to create a `requirements.txt` from a `uv.lock` -- it's just using a different interface to install the given dependencies.)

---

_Comment by @kurt-rhee on 2025-04-09 20:00_

Super helpful answer, thank you!

---

_Closed by @kurt-rhee on 2025-04-09 20:00_

---

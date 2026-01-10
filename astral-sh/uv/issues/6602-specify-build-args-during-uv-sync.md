---
number: 6602
title: Specify build args during uv sync
type: issue
state: closed
author: ion-elgreco
labels:
  - question
assignees: []
created_at: 2024-08-25T13:00:58Z
updated_at: 2024-12-02T13:48:52Z
url: https://github.com/astral-sh/uv/issues/6602
synced_at: 2026-01-10T01:24:03Z
---

# Specify build args during uv sync

---

_Issue opened by @ion-elgreco on 2024-08-25 13:00_

When we run uv sync it builds the package but sometimes we want to specify build args to the underlying build tool. Is there any way to allow this passthrough at the moment?


---

_Comment by @charliermarsh on 2024-08-25 15:53_

Have you seen https://docs.astral.sh/uv/reference/settings/#config-settings?

---

_Label `question` added by @charliermarsh on 2024-08-25 15:53_

---

_Comment by @charliermarsh on 2024-09-06 01:45_

Closing for lack of follow-up but happy to answer any further questions.

---

_Closed by @charliermarsh on 2024-09-06 01:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 01:45_

---

_Comment by @ion-elgreco on 2024-12-02 08:40_

@charliermarsh but how can you make this dynamic? I want to specify new build_args whenver

---

_Comment by @charliermarsh on 2024-12-02 13:32_

Can you expand on your comment? You can just pass these via the CLI.

---

_Comment by @ion-elgreco on 2024-12-02 13:48_

> Can you expand on your comment? You can just pass these via the CLI.

Ah you mean like uv build -C ?

---

_Comment by @charliermarsh on 2024-12-02 13:48_

Yeah

---

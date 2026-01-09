---
number: 12609
title: Disable auditing when installing
type: issue
state: closed
author: AustinScola
labels:
  - question
assignees: []
created_at: 2025-04-01T21:00:11Z
updated_at: 2025-06-27T09:25:53Z
url: https://github.com/astral-sh/uv/issues/12609
synced_at: 2026-01-07T13:12:18-06:00
---

# Disable auditing when installing

---

_Issue opened by @AustinScola on 2025-04-01 21:00_

### Question

How can I disable auditing when I run `uv pip install`?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @AustinScola on 2025-04-01 21:00_

---

_Comment by @zanieb on 2025-04-01 21:13_

Can you say more please?

---

_Comment by @Techcable on 2025-06-27 00:14_

If I run `uv pip install` and the packages are already installed, I see a mesage about "auditing":

![Image](https://github.com/user-attachments/assets/1a7bf5d8-6681-4341-9baa-e953a70eec82)


When I type `uv sync` on an up-to-date venv, I see a similar mention of "auditing":

![Image](https://github.com/user-attachments/assets/8584baa9-6442-436b-ac1e-88cce4b30410)

What does "auditing" mean here? Does it make sense for it to be disabled?

---

_Comment by @charliermarsh on 2025-06-27 00:15_

Oh, it just means that we verified that the appropriate packages are installed (i.e., there was "nothing to do" for those 17 packages).

---

_Comment by @Techcable on 2025-06-27 00:17_

> Oh, it just means that we verified that the appropriate packages are installed (i.e., there was "nothing to do" for those 17 packages).

Thank you for the prompt response :)

---

_Closed by @konstin on 2025-06-27 09:25_

---

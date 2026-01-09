---
number: 11242
title: "Invalid checkout setting ``fetch-depth`` in ``publish-docs`` action"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2025-02-05T12:24:03Z
updated_at: 2025-02-05T16:13:47Z
url: https://github.com/astral-sh/uv/issues/11242
synced_at: 2026-01-07T13:12:18-06:00
---

# Invalid checkout setting ``fetch-depth`` in ``publish-docs`` action

---

_Issue opened by @FishAlchemist on 2025-02-05 12:24_

### Summary
This is actually part of #11148, but I don't know how to fix this issue, so I'm opening an issue to let you know.
``git-revision-date-localized-plugin`` requires fetch-depth to be set to 0, and it is written in the configuration file, but the actual operation is set to fail.
https://github.com/timvink/mkdocs-git-revision-date-localized-plugin?tab=readme-ov-file#note-when-using-build-systems-like-github-actions
* **Settings location**
https://github.com/astral-sh/uv/blob/49b85d2e65f82411b76d6ef3a68b3c20fad197f6/.github/workflows/publish-docs.yml#L26-L29
* **The current running results**
![Image](https://github.com/user-attachments/assets/02735072-9cba-4104-ba35-cb8e646626ee)
![Image](https://github.com/user-attachments/assets/3921e973-69e0-4e8f-9534-b9bdcb71917c)

### Platform

Based on the current ``Github Actions`` configuration.

### Version

Unrelated

### Python version

Based on the current ``Github Actions`` configuration.

---

_Label `bug` added by @FishAlchemist on 2025-02-05 12:24_

---

_Comment by @charliermarsh on 2025-02-05 14:29_

What is the net effect here? Are the revision dates wrong?

---

_Comment by @FishAlchemist on 2025-02-05 14:36_

Due to shallow fetching, it is not possible to retrieve the commit history for every file, which results in the inability to read the dates.
The deployed document website currently lacks the 'last updated' timestamp.
![Image](https://github.com/user-attachments/assets/62c2a880-ab3d-4672-8207-1956f44247d2)

---

_Comment by @zanieb on 2025-02-05 15:02_

Huh thanks for following up on that. It looks correct in the release though

<img width="1741" alt="Image" src="https://github.com/user-attachments/assets/e8b369b3-a220-4b04-9323-e8814aa89333" />

---

_Comment by @FishAlchemist on 2025-02-05 15:35_

@zanieb The issue is likely that the ``mkdocs.template.yml``  is missing the ``git-revision-date-localized-plugin`` plugin.

![Image](https://github.com/user-attachments/assets/2982e1c3-93ae-4f93-a166-3975947cc872)

https://github.com/astral-sh/uv/blob/6f8d9b85d8cdd41b03669dda9374f76ab7b2c7e4/mkdocs.insiders.yml#L1
I previously considered putting the ``git-revision-date-localized-plugin`` in ``mkdocs.template.yml``, but unfortunately it didn't work when placed there.

---

_Comment by @zanieb on 2025-02-05 15:39_

Oh, you only put it in the non-insiders file? :D that explains that

---

_Referenced in [astral-sh/uv#11246](../../astral-sh/uv/pulls/11246.md) on 2025-02-05 15:43_

---

_Comment by @FishAlchemist on 2025-02-05 15:44_

That seems to be a problem for insiders only. I don't have the permissions to install and update dependencies, so there's no way for me to test and modify them.

---

_Comment by @zanieb on 2025-02-05 15:46_

Yeah no problem — we use insiders for our production docs so those need to be updated for things to take effect.

---

_Comment by @FishAlchemist on 2025-02-05 15:56_

![Image](https://github.com/user-attachments/assets/18c95e4b-0c56-4bba-bddd-36fcfc07a939)
By the way, have you considered allowing non-release CI to also use ``fetch-depth: 0``? Otherwise, these types of issues can only be tested successfully in releases

---

_Comment by @zanieb on 2025-02-05 16:06_

You just didn't change it there https://github.com/astral-sh/uv/blob/c54dbcbcc250721e73c35a40e454b7205f90ec4d/.github/workflows/ci.yml#L399

---

_Comment by @zanieb on 2025-02-05 16:06_

Adding in https://github.com/astral-sh/uv/pull/11246/commits/ea126ba120833ffcae9bd4873dd9c117f5b97206

---

_Closed by @zanieb on 2025-02-05 16:13_

---

_Closed by @zanieb on 2025-02-05 16:13_

---

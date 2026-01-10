---
number: 1120
title: Restore Git progress bars
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - wish
  - tracing
assignees: []
created_at: 2024-01-26T13:29:37Z
updated_at: 2024-07-02T01:35:55Z
url: https://github.com/astral-sh/uv/issues/1120
synced_at: 2026-01-10T01:23:05Z
---

# Restore Git progress bars

---

_Issue opened by @charliermarsh on 2024-01-26 13:29_

Cargo has these. I disabled them because it requires threading down our printer abstraction.

---

_Label `enhancement` added by @charliermarsh on 2024-01-26 13:29_

---

_Label `wish` added by @charliermarsh on 2024-01-26 13:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 17:16_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-01-27 01:54_

---

_Comment by @zanieb on 2024-07-01 21:35_

@ibraheemdev I presume these are not included now that we're using the git CLI? Is it even possible anymore?

---

_Label `tracing` added by @zanieb on 2024-07-01 21:35_

---

_Comment by @ibraheemdev on 2024-07-01 21:38_

We could possibly stream the git CLI output and convert it to our own progress bars, but that doesn't seem very stable. We could also monitor the size of the `.git` directory, though I'm not sure we be able to get a size estimate.

We can maybe add an option to not hide the `git fetch` output?

---

_Comment by @zanieb on 2024-07-02 01:35_

Let's just close this for now, though it seems fair to want to see some sort of git output eventually.

---

_Closed by @zanieb on 2024-07-02 01:35_

---

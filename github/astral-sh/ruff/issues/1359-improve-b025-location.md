---
number: 1359
title: Improve B025 location
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2022-12-24T16:16:54Z
updated_at: 2022-12-24T17:22:12Z
url: https://github.com/astral-sh/ruff/issues/1359
synced_at: 2026-01-07T13:12:14-06:00
---

# Improve B025 location

---

_Issue opened by @harupy on 2022-12-24 16:16_

### Current

![image](https://user-images.githubusercontent.com/17039389/209443467-d1c72776-e8e9-47db-a327-24d25b7878e9.png)

- B025 is raised against the try statement
- Difficult to identify duplicate locations

### Proposed

![image](https://user-images.githubusercontent.com/17039389/209443473-f6d83ba3-6378-46d4-9cdd-338ff7e3ab32.png)

- B025 is raised against each duplicate
- Easy to identify duplicate locations

---

_Comment by @harupy on 2022-12-24 16:19_

Happy to file a PR if the proposal sounds reasonable.

---

_Comment by @charliermarsh on 2022-12-24 16:19_

Sounds good to me!

---

_Referenced in [astral-sh/ruff#1360](../../astral-sh/ruff/pulls/1360.md) on 2022-12-24 16:25_

---

_Closed by @charliermarsh on 2022-12-24 17:22_

---

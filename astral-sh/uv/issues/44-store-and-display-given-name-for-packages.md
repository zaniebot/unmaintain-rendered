---
number: 44
title: "Store and display \"given name\" for packages"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-07T19:08:29Z
updated_at: 2023-10-24T19:23:20Z
url: https://github.com/astral-sh/uv/issues/44
synced_at: 2026-01-10T01:23:03Z
---

# Store and display "given name" for packages

---

_Issue opened by @charliermarsh on 2023-10-07 19:08_

Right now, we use the normalized name everywhere. But if the user or package author writes `Jinja2`, we should show that instead.

---

_Comment by @konstin on 2023-10-09 12:08_

Note that there isn't one canonical name, there might be multiple forms in use at the same time (even between wheel filename and dist-info dir), we should just pick one and run with that

---

_Comment by @charliermarsh on 2023-10-10 20:07_

Unsure on whether we actually want to change anything here. `pip-compile` seems to always use normalized names, `pip` sometimes shows the non-normalized versions (e.g., if you `pip install Flask`, it will show `Flask`, and also `Jinja2`, because that's how Flask styles their dependency in the metadata).

---

_Closed by @charliermarsh on 2023-10-24 19:23_

---

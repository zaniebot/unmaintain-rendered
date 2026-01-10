```yaml
number: 6372
title: "Improve interactions between `.python-version` files and project `requires-python`"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: zb/version-file-discover
head: zb/version-file-discover-project
created_at: 2024-08-21T19:55:33Z
updated_at: 2024-10-04T18:07:31Z
url: https://github.com/astral-sh/uv/pull/6372
synced_at: 2026-01-10T12:53:33Z
```

# Improve interactions between `.python-version` files and project `requires-python`

---

_Pull request opened by @zanieb on 2024-08-21 19:55_

Closes https://github.com/astral-sh/uv/issues/5228
Required for https://github.com/astral-sh/uv/pull/6370

This ensures that we

1. Ignore`.python-version` files outside of projects if they specify a version that's incompatible with the `requires-python`
2. Warn when `.python-version` files inside a project are incompatible with the `requires-python`

Unfortunately, this totally falls apart if the `.python-version` file isn't a version specifier, e.g., if it's an interpreter path.

This begins to create a unified place to determine the `PythonRequest` to use — it's getting really complicated.

---

_@zanieb reviewed on 2024-08-21 20:00_

---

_Review comment by @zanieb on `crates/uv/tests/python_find.rs`:303 on 2024-08-21 20:00_

Oh, we won't even let you write this pin! But we do need to handle the case where you've written it previously or with another tool.

---

_Comment by @aspeddro on 2024-09-26 01:23_

With this PR a project with a `.python-version` different from `requires-python`, `requires-python` is used and `.python-version` is ignored?


I think it makes sense that `uv run example.py` ignores `.python-version`
![image](https://github.com/user-attachments/assets/1cc3ea15-3e9f-4ad6-a50c-9cee7a5a6056)


---

_Comment by @zanieb on 2024-09-26 01:52_

> With this PR a project with a .python-version different from requires-python, requires-python is used and .python-version is ignored?

Basically, yes. We will do our best to respect `.python-version` but if it's not compatible with the project or script we will warn and ignore it.

---

_Closed by @zanieb on 2024-10-04 18:07_

---

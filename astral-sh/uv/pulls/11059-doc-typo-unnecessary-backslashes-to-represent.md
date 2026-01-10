```yaml
number: 11059
title: "doc typo: unnecessary backslashes to represent brackets in markdown"
type: pull_request
state: merged
author: aureliobarbosa
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-01-29T12:56:14Z
updated_at: 2025-01-29T14:17:31Z
url: https://github.com/astral-sh/uv/pull/11059
synced_at: 2026-01-10T11:45:26Z
```

# doc typo: unnecessary backslashes to represent brackets in markdown

---

_Pull request opened by @aureliobarbosa on 2025-01-29 12:56_

There is a small typo in the doc which could mislead users: reference to a table in `pyproject.toml` currently appears as [`\[project.entry-points\]`](https://packaging.python.org/en/latest/guides/creating-and-discovering-plugins/#using-package-metadata) while it should be  [`[project.entry-points]`](https://packaging.python.org/en/latest/guides/creating-and-discovering-plugins/#using-package-metadata).

Backslashes are appearing because they weren't supposed to be used on code representation in Markdown.


---

_Renamed from "typo: unnecessary backslashes to represent brackets in markdown" to "doc typo: unnecessary backslashes to represent brackets in markdown" by @aureliobarbosa on 2025-01-29 12:56_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-29 14:06_

---

_Label `documentation` added by @charliermarsh on 2025-01-29 14:06_

---

_@zanieb approved on 2025-01-29 14:16_

---

_Comment by @zanieb on 2025-01-29 14:17_

Thanks!

---

_Merged by @zanieb on 2025-01-29 14:17_

---

_Closed by @zanieb on 2025-01-29 14:17_

---

```yaml
number: 6986
title: "docs(configuration): clarify `uv.toml` precedence"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/clarify-uv-toml-precedence
created_at: 2024-09-03T23:02:12Z
updated_at: 2024-09-04T16:56:15Z
url: https://github.com/astral-sh/uv/pull/6986
synced_at: 2026-01-10T12:53:37Z
```

# docs(configuration): clarify `uv.toml` precedence

---

_Pull request opened by @mkniewallner on 2024-09-03 23:02_

## Summary

Add a note in the documentation to clarify that `uv.toml` files take precedence over `[tool.uv]` section in `pyproject.toml`, based on the warning shown in the CLI:

```console
$ uv version
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
uv 0.4.2 (Homebrew 2024-09-01)
```

---

_Marked ready for review by @mkniewallner on 2024-09-03 23:03_

---

_Assigned to @charliermarsh by @zanieb on 2024-09-03 23:14_

---

_Label `documentation` added by @zanieb on 2024-09-03 23:14_

---

_@charliermarsh approved on 2024-09-04 15:56_

Thanks!

---

_Merged by @charliermarsh on 2024-09-04 15:57_

---

_Closed by @charliermarsh on 2024-09-04 15:57_

---

_Branch deleted on 2024-09-04 16:56_

---

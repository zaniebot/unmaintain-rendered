```yaml
number: 2492
title: "Add support for Hatch's `{root:uri}` paths in editable installs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/b
created_at: 2024-03-16T18:37:26Z
updated_at: 2024-03-16T19:06:43Z
url: https://github.com/astral-sh/uv/pull/2492
synced_at: 2026-01-10T14:49:08Z
```

# Add support for Hatch's `{root:uri}` paths in editable installs

---

_Pull request opened by @charliermarsh on 2024-03-16 18:37_

## Summary

If a package uses Hatch's `root.uri` feature, we currently error:

```toml
dependencies = [
  "black @ {root:uri}/../black_editable"
]
```

Even though we're using PEP 517 hooks to get the metadata, which _should_ support this. The problem is that we load the full `PyProjectToml`, which means we parse the requirements, which means we reject what looks like a relative URL in dependencies.

Instead, we should only enforce a limited subset of `pyproject.toml` (arguably none).

Closes https://github.com/astral-sh/uv/issues/2475.


---

_Label `compatibility` added by @charliermarsh on 2024-03-16 18:37_

---

_Renamed from "Avoid enforcing pyproject.toml correctness in build hooks" to "Add support for Hatch's `{root:uri}` paths in editable installs" by @charliermarsh on 2024-03-16 18:37_

---

_Merged by @charliermarsh on 2024-03-16 19:06_

---

_Closed by @charliermarsh on 2024-03-16 19:06_

---

_Branch deleted on 2024-03-16 19:06_

---

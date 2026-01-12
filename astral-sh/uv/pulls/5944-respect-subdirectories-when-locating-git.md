```yaml
number: 5944
title: Respect subdirectories when locating Git workspaces
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sub
created_at: 2024-08-09T00:03:20Z
updated_at: 2024-08-09T00:13:25Z
url: https://github.com/astral-sh/uv/pull/5944
synced_at: 2026-01-12T16:07:07Z
```

# Respect subdirectories when locating Git workspaces

---

_@charliermarsh_

## Summary

We were discovering the workspace from the Git repository root, so attempting to build any subdirectories would fail.

Closes https://github.com/astral-sh/uv/issues/5942.

## Test Plan

```
cargo run pip install \
	git+https://github.com/flyteorg/flytekit.git@master#subdirectory=plugins/flytekit-flyteinteractive
```


---

_Comment by @charliermarsh on 2024-08-09 00:12_

It's so hard to construct a test for this.

---

_Comment by @charliermarsh on 2024-08-09 00:13_

You need:

- A Git repository whose name also exists as a PyPI package.
- With a `pyproject.toml` at the root.
- With a subdirectory containing another package.
- And that sub-package needs to declare a dependency on the package root.

---

_Label `bug` added by @charliermarsh on 2024-08-09 00:13_

---

_Merged by @charliermarsh on 2024-08-09 00:13_

---

_Closed by @charliermarsh on 2024-08-09 00:13_

---

_Branch deleted on 2024-08-09 00:13_

---

_Comment by @charliermarsh on 2024-08-09 00:13_

I tried but gave up.

---

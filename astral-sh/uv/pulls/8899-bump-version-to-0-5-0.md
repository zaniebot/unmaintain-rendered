```yaml
number: 8899
title: Bump version to 0.5.0
type: pull_request
state: merged
author: zanieb
labels:
  - no-build
  - releases
assignees: []
merged: true
base: main
head: zb/050
created_at: 2024-11-07T21:27:13Z
updated_at: 2024-11-07T22:38:26Z
url: https://github.com/astral-sh/uv/pull/8899
synced_at: 2026-01-10T12:00:00Z
```

# Bump version to 0.5.0

---

_Pull request opened by @zanieb on 2024-11-07 21:27_

_No description provided._

---

_Label `releases` added by @zanieb on 2024-11-07 21:27_

---

_@charliermarsh reviewed on 2024-11-07 21:57_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:5 on 2024-11-07 21:57_

In the Ruff post, Alex includes:

> most of our users should be able to upgrade to the latest Ruff version without having to make any significant edits to their code or configuration. But with that said, here are a few things to keep in mind when upgrading:

---

_@charliermarsh reviewed on 2024-11-07 21:58_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:11 on 2024-11-07 21:58_

I think the last sentence is confusing because it makes it sound like that's the new behavior. But the new behavior is finding in parents -- it just doesn't go past project boundaries.

---

_@charliermarsh reviewed on 2024-11-07 21:58_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:15 on 2024-11-07 21:58_

Let's use "enable" instead of "allow".

---

_@charliermarsh approved on 2024-11-07 21:59_

---

_Review comment by @zanieb on `CHANGELOG.md`:15 on 2024-11-07 22:01_

But it's `--prerelease allow`

---

_@zanieb reviewed on 2024-11-07 22:01_

---

_Label `no-build` added by @zanieb on 2024-11-07 22:13_

---

_Review comment by @edmorley on `CHANGELOG.md`:51 on 2024-11-07 22:21_

```suggestion
  Previously, uv would create a `.python-version` file for workspace members during `uv init`. Now, uv will only do so if the version differs from the `.python-version` file in the workspace root since uv will respect `.python-version` files in parent directories.
```

---

_@edmorley reviewed on 2024-11-07 22:25_

---

_Merged by @zanieb on 2024-11-07 22:38_

---

_Closed by @zanieb on 2024-11-07 22:38_

---

_Branch deleted on 2024-11-07 22:38_

---

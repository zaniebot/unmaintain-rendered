```yaml
number: 9644
title: Include more sources to avoid lowest bound warning
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-warn-for-more-bound-source
created_at: 2024-12-04T17:55:19Z
updated_at: 2024-12-05T09:09:40Z
url: https://github.com/astral-sh/uv/pull/9644
synced_at: 2026-01-10T12:00:00Z
```

# Include more sources to avoid lowest bound warning

---

_Pull request opened by @konstin on 2024-12-04 17:55_

In https://github.com/astral-sh/uv/issues/8155#issuecomment-2508969900, resolution lowest was complaining about missing lower bounds for a pacakge, even though the package had a URL, too:

```
uv pip install dist/pymatgen-2024.10.3.tar.gz pymatgen[ci,optional] --resolution=lowest
```

The error was raised from `pymatgen[ci,optional]`, because we were looking at it before looking at the "URL" `dist/pymatgen-2024.10.3.tar.gz`.

I've also added constraints and overrides to the bounds lookup, since they are missing from the dependency graph.

Fixes #8155 (again)


---

_Renamed from "Include more source for lowest bound warning." to "Include more source for lowest bound warning" by @konstin on 2024-12-04 17:55_

---

_Label `bug` added by @konstin on 2024-12-04 17:55_

---

_Renamed from "Include more source for lowest bound warning" to "Include more sources to avoid lowest bound warning" by @konstin on 2024-12-04 17:55_

---

_@zanieb approved on 2024-12-04 18:30_

---

_@charliermarsh reviewed on 2024-12-04 18:31_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2330 on 2024-12-04 18:31_

This is quadratic, right?

---

_@konstin reviewed on 2024-12-04 19:36_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2330 on 2024-12-04 19:36_

Forgot to add the comment to the PR: This only runs for direct deps, so the quadratic loop runs about once, and in the number of direct dependencies.

---

_Merged by @konstin on 2024-12-05 09:09_

---

_Closed by @konstin on 2024-12-05 09:09_

---

_Branch deleted on 2024-12-05 09:09_

---

```yaml
number: 11496
title: Bump version to v0.6.0
type: pull_request
state: merged
author: zanieb
labels:
  - no-build
  - releases
  - no-test
assignees: []
merged: true
base: main
head: zb/060-cl
created_at: 2025-02-13T23:07:10Z
updated_at: 2025-02-14T17:55:57Z
url: https://github.com/astral-sh/uv/pull/11496
synced_at: 2026-01-12T16:09:52Z
```

# Bump version to v0.6.0

---

_@zanieb_

_No description provided._

---

_Label `releases` added by @zanieb on 2025-02-13 23:07_

---

_Renamed from "Generate changelog for v0.6.0" to "Bump version to v0.6.0" by @zanieb on 2025-02-13 23:07_

---

_Label `no-build` added by @zanieb on 2025-02-13 23:20_

---

_@charliermarsh reviewed on 2025-02-13 23:36_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:11 on 2025-02-13 23:36_

```suggestion
  Previously, `uv init` created a `hello.py` sample file. Now, `uv init` will create `main.py` instead — which aligns with expectations from user feedback. The `--bare` option can be used to avoid creating the file altogether.
```

---

_Review comment by @charliermarsh on `CHANGELOG.md`:17 on 2025-02-13 23:36_

(I actually didn't follow this, what's it for?)

---

_@charliermarsh reviewed on 2025-02-13 23:36_

---

_@charliermarsh approved on 2025-02-13 23:37_

---

_@charliermarsh reviewed on 2025-02-13 23:38_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:23 on 2025-02-13 23:38_

```suggestion
  Previously, uv would silently ignore non-existent extras requested on the command-line (e.g., via `uv sync --extra foo`). This is _generally_ correct behavior when resolving requests for package extras, because an extra may be present on one compatible version of a package but not another. However, this flexibility doesn't need to apply to the local project and it's less surprising to error here.
```

---

_@charliermarsh reviewed on 2025-02-13 23:38_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:23 on 2025-02-13 23:38_

Not necessarily better, I just found ", would not fail" to be a little awkward.

---

_@zanieb reviewed on 2025-02-13 23:40_

---

_Review comment by @zanieb on `CHANGELOG.md`:17 on 2025-02-13 23:40_

Ah this was for https://github.com/astral-sh/uv/issues/8775 and the Ray team

---

_@zanieb reviewed on 2025-02-13 23:41_

---

_Review comment by @zanieb on `CHANGELOG.md`:23 on 2025-02-13 23:41_

I wasn't sure if we warned in some contexts.

---

_@cthoyt reviewed on 2025-02-14 09:40_

---

_Review comment by @cthoyt on `scripts/packages/built-by-uv/pyproject.toml`:27 on 2025-02-14 09:40_

should the minimum version become 0.6 now?

---

_@zanieb reviewed on 2025-02-14 13:11_

---

_Review comment by @zanieb on `scripts/packages/built-by-uv/pyproject.toml`:27 on 2025-02-14 13:11_

I don't think it needs to.

---

_Label `no-test` added by @zanieb on 2025-02-14 17:14_

---

_Merged by @zanieb on 2025-02-14 17:55_

---

_Closed by @zanieb on 2025-02-14 17:55_

---

_Branch deleted on 2025-02-14 17:55_

---

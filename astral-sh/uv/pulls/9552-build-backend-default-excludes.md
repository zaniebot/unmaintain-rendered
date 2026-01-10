```yaml
number: 9552
title: "Build backend: Default excludes"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/default-exclude
created_at: 2024-12-01T11:35:04Z
updated_at: 2024-12-03T14:18:11Z
url: https://github.com/astral-sh/uv/pull/9552
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Default excludes

---

_Pull request opened by @konstin on 2024-12-01 11:35_

When adding excludes, we usually don't want to include python cache files. On the contrary, I haven't seen any project in my ecosystem research that would want any of `__pycache__`, `*.pyc`, `*.pyo` to be included. By moving them behind a `default-excludes` toggle, they are always active even when defining custom excludes, but can be deactivated if the user so chooses.

With includes and excludes being this small again, we can roll back the include-exclude anchored difference to always using anchored globs (i.e. you would need to use `**/build-*.h` below).

A pyproject.toml with custom settings with the change applied:

```toml
[project]
name = "foo"
version = "0.1.0"
readme = "README.md"
license-files = ["LICENSE*", "third-party-licenses/*"]

[tool.uv.build-backend]
# A file we need for the source dist -> wheel step, but not in the wheel itself (currently unused)
source-include = ["data/build-script.py"]
# A temporary or generated file we want to ignore
source-exclude = ["/src/foo/not-packaged.txt"]
# Headers are build-only
wheel-exclude = ["build-*.h"]

[tool.uv.build-backend.data]
scripts = "scripts"
data = "assets"
headers = "header"

[build-system]
requires = ["uv>=0.5.5,<0.6"]
build-backend = "uv"
```

---

_Label `preview` added by @konstin on 2024-12-01 11:35_

---

_@konstin reviewed on 2024-12-01 11:35_

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:738 on 2024-12-01 11:35_

I'm staging the real docs here

---

_Review requested from @BurntSushi by @konstin on 2024-12-01 11:35_

---

_Review requested from @MichaReiser by @konstin on 2024-12-01 11:35_

---

_Merged by @konstin on 2024-12-01 13:09_

---

_Closed by @konstin on 2024-12-01 13:09_

---

_Branch deleted on 2024-12-01 13:09_

---

_@BurntSushi reviewed on 2024-12-03 14:18_

Makes sense to me.

---

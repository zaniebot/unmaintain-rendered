```yaml
number: 13927
title: Filter Python executables on Windows by Python version metadata
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - performance
  - windows
assignees: []
created_at: 2025-06-09T16:58:55Z
updated_at: 2025-06-12T19:42:11Z
url: https://github.com/astral-sh/uv/issues/13927
synced_at: 2026-01-12T16:01:39Z
```

# Filter Python executables on Windows by Python version metadata

---

_@zanieb_

See https://github.com/astral-sh/ruff/pull/18550#issuecomment-2956350021

Windows Python executables seem to have the Python version embedded in their metadata. This may be faster than querying the Python interpreter to get its version, which would speed up Python discovery on Windows when a specific version is requested.

---

_Label `help wanted` added by @zanieb on 2025-06-09 16:58_

---

_Label `performance` added by @zanieb on 2025-06-09 16:58_

---

_Label `windows` added by @AlexWaygood on 2025-06-09 17:00_

---

_Comment by @zanieb on 2025-06-09 20:11_

See https://docs.rs/pelite/latest/pelite/resources/version_info/struct.VersionInfo.html

I don't know if we want to add that as a dependency, but the `object` crate (which we use for `PeFile` currently) doesn't seem to include resource parsing.

---

_Comment by @zanieb on 2025-06-12 19:42_

I took a shot at this in https://github.com/astral-sh/uv/pull/13948 but ran into problems and don't have a Windows setup to debug easily.

---

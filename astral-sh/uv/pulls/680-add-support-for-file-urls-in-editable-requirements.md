```yaml
number: 680
title: "Add support for `file://` URLs in editable requirements"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/editable-url
created_at: 2023-12-18T03:24:05Z
updated_at: 2023-12-18T14:55:39Z
url: https://github.com/astral-sh/uv/pull/680
synced_at: 2026-01-12T16:04:07Z
```

# Add support for `file://` URLs in editable requirements

---

_@charliermarsh_

_No description provided._

---

_Review requested from @konstin by @charliermarsh on 2023-12-18 03:24_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:552 on 2023-12-18 09:36_

This is us supporting less features than pip

```suggestion
                write!(f, "Unsupported URL, only `file://` URLs are supported: {url}")
```

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:598 on 2023-12-18 09:37_

```suggestion
                    "Unsupported URL in {}, only `file://` URLs are supported: {}",
```

---

_@konstin approved on 2023-12-18 09:37_

---

_Merged by @charliermarsh on 2023-12-18 14:55_

---

_Closed by @charliermarsh on 2023-12-18 14:55_

---

_Branch deleted on 2023-12-18 14:55_

---

```yaml
number: 14573
title: "Allow `--config-settings-package` to apply configuration settings at the package level"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/config-settings-package
created_at: 2025-07-11T23:51:29Z
updated_at: 2025-07-18T01:27:55Z
url: https://github.com/astral-sh/uv/pull/14573
synced_at: 2026-01-12T16:11:17Z
```

# Allow `--config-settings-package` to apply configuration settings at the package level

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/14564.

Closes https://github.com/astral-sh/uv/issues/10940.


---

_Label `enhancement` added by @charliermarsh on 2025-07-11 23:51_

---

_Marked ready for review by @charliermarsh on 2025-07-11 23:51_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-12 00:20_

---

_@zanieb reviewed on 2025-07-17 16:16_

---

_Review comment by @zanieb on `docs/reference/settings.md`:1026 on 2025-07-17 16:16_

The name is inverted in the example.

---

_@zanieb reviewed on 2025-07-17 16:17_

---

_Review comment by @zanieb on `docs/reference/settings.md`:1026 on 2025-07-17 16:17_

Your test also uses

```
        [tool.uv.config-settings-package]
        setuptools = { editable_mode = "compat" }
```

should we prefer that format?

---

_@zanieb approved on 2025-07-17 16:17_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:1026 on 2025-07-17 16:44_

Yeah, good call.

---

_@charliermarsh reviewed on 2025-07-17 16:44_

---

_Merged by @charliermarsh on 2025-07-18 01:27_

---

_Closed by @charliermarsh on 2025-07-18 01:27_

---

_Branch deleted on 2025-07-18 01:27_

---

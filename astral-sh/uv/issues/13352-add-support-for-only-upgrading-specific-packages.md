```yaml
number: 13352
title: "Add support for only upgrading specific packages, i.e., `--only-upgrade-package` or `--upgrade-mode strict`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-05-08T18:11:44Z
updated_at: 2025-05-08T18:12:05Z
url: https://github.com/astral-sh/uv/issues/13352
synced_at: 2026-01-12T16:01:26Z
```

# Add support for only upgrading specific packages, i.e., `--only-upgrade-package` or `--upgrade-mode strict`

---

_@zanieb_

### Summary

In this mode, all packages that need to a version change must be specified explicitly. The resolver should enforce exact pinning version semantics for all other packages. If additional package versions need to change, uv should exit with an error.

### Example

See https://github.com/astral-sh/uv/issues/13347 for discussion.

---

_Label `enhancement` added by @zanieb on 2025-05-08 18:11_

---

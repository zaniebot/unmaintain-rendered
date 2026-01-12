```yaml
number: 473
title: Build source distributions in parallel in pip-sync
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - wish
assignees: []
created_at: 2023-11-21T12:41:02Z
updated_at: 2023-11-24T17:47:59Z
url: https://github.com/astral-sh/uv/issues/473
synced_at: 2026-01-12T15:58:23Z
```

# Build source distributions in parallel in pip-sync

---

_@konstin_

We currently first download wheels and source dists and then build source dists sequentially in the installer:

https://github.com/astral-sh/puffin/blob/35fd86631b6a57e5c453055cad159abd672e20ee/crates/puffin-cli/src/commands/pip_sync.rs#L234-L237

We should download and built them in parallel

---

_Label `enhancement` added by @konstin on 2023-11-21 12:41_

---

_Label `wish` added by @konstin on 2023-11-21 12:41_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-21 12:54_

---

_Closed by @charliermarsh on 2023-11-24 17:47_

---

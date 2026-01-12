```yaml
number: 3244
title: Upgrade packse to 0.3.15
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/packse-upgrade
created_at: 2024-04-24T15:29:57Z
updated_at: 2024-04-24T15:54:04Z
url: https://github.com/astral-sh/uv/pull/3244
synced_at: 2026-01-12T16:05:31Z
```

# Upgrade packse to 0.3.15

---

_@zanieb_

Includes https://github.com/astral-sh/packse/pull/179 for #3225

```
uv pip compile scripts/scenarios/requirements.in -o scripts/scenarios/requirements.txt --refresh-package packse --upgrade
cargo dev fetch-python
source .env
./scripts/sync_scenarios.sh
```

---

_Label `testing` added by @zanieb on 2024-04-24 15:30_

---

_Review requested from @ibraheemdev by @zanieb on 2024-04-24 15:30_

---

_Comment by @konstin on 2024-04-24 15:31_

This needs a `BUILD_VENDOR_LINKS_URL` update too, should we add that to the script?

---

_Comment by @zanieb on 2024-04-24 15:31_

Ah thanks for the note. We probably should!

---

_Comment by @zanieb on 2024-04-24 15:38_

We could add a separate `upgrade-packse` script that runs the requirements update and does this replacement?

---

_@ibraheemdev approved on 2024-04-24 15:45_

Looks good after `BUILD_VENDOR_LINKS_URL` is updated.

---

_Merged by @zanieb on 2024-04-24 15:54_

---

_Closed by @zanieb on 2024-04-24 15:54_

---

_Branch deleted on 2024-04-24 15:54_

---

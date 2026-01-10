```yaml
number: 9318
title: "Remove `--upgrade`, `--no-upgrade`, and `--upgrade-package` from `uv tool upgrade`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2024-11-21T14:21:48Z
updated_at: 2024-11-21T14:35:59Z
url: https://github.com/astral-sh/uv/pull/9318
synced_at: 2026-01-10T12:00:00Z
```

# Remove `--upgrade`, `--no-upgrade`, and `--upgrade-package` from `uv tool upgrade`

---

_Pull request opened by @charliermarsh on 2024-11-21 14:21_

## Summary

`--upgrade` isn't useful, since it's the default. So it's now hidden, but continues to warn if you enable it.

`--no-upgrade` isn't useful, since it panics. So it's now removed entirely. This isn't breaking, since it already didn't work.

`--upgrade-package` actually _is_ useful, because it turns out it allows things like: `uv tool upgrade babel --upgrade-package "babel<0.2.14"` to constrain the upgrade.

I left this in place but hid it... I think we should provide a better workflow for this, like `uv tool upgrade "babel<0.2.14"`? It's strange to specify the package twice, and that `uv tool upgrade` has an `--upgrade-package` flag.

Closes https://github.com/astral-sh/uv/issues/9317.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-21 14:21_

---

_Label `bug` added by @charliermarsh on 2024-11-21 14:21_

---

_@zanieb approved on 2024-11-21 14:25_

Sort of tragic that it's so hard to share the options

---

_Comment by @charliermarsh on 2024-11-21 14:26_

Do you think the changes themselves make sense? It's tricky, I wasn't sure.

---

_Merged by @charliermarsh on 2024-11-21 14:35_

---

_Closed by @charliermarsh on 2024-11-21 14:35_

---

_Branch deleted on 2024-11-21 14:35_

---

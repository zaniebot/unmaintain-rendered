```yaml
number: 691
title: Add lock around virtual environment
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-12-18T18:15:56Z
updated_at: 2023-12-18T21:44:47Z
url: https://github.com/astral-sh/uv/issues/691
synced_at: 2026-01-10T05:40:31Z
```

# Add lock around virtual environment

---

_Issue opened by @charliermarsh on 2023-12-18 18:15_

Like Cargo, we should probably disallow concurrent modification of the same virtualenv.

---

_Label `enhancement` added by @charliermarsh on 2023-12-18 18:15_

---

_Comment by @konstin on 2023-12-18 18:17_

`InstallLocation::acquire_lock` does that.

---

_Comment by @charliermarsh on 2023-12-18 18:19_

Nice. It looks like we aren't using this for uninstalls yet (brief skim of the code).

---

_Comment by @charliermarsh on 2023-12-18 19:39_

I actually think `InstallLocation::acquire_lock` may not be sufficient because we assume that the contents don't change for the duration of the command, not just for a single install within the command.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-18 19:39_

---

_Closed by @charliermarsh on 2023-12-18 21:44_

---

```yaml
number: 6721
title: document --reinstall with --exclude-newer to ensure downgrades
type: pull_request
state: merged
author: davidszotten
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-exclude-newer-downgrade
created_at: 2024-08-27T20:06:39Z
updated_at: 2024-10-09T23:03:32Z
url: https://github.com/astral-sh/uv/pull/6721
synced_at: 2026-01-10T12:53:34Z
```

# document --reinstall with --exclude-newer to ensure downgrades

---

_Pull request opened by @davidszotten on 2024-08-27 20:06_

fixes #6676

---

_@zanieb reviewed on 2024-08-27 20:30_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:301 on 2024-08-27 20:30_

I think I'd say...

```suggestion
!!! note

    The `--exclude-newer` option is only applied to registry packages and will not downgrade
    previously installed packages unless the `--reinstall` flag is provided, in which case uv will
    perform a new resolution.
```

wdyt? 

cc @charliermarsh 

---

_Label `documentation` added by @zanieb on 2024-08-27 21:10_

---

_@zanieb reviewed on 2024-08-27 21:11_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:301 on 2024-08-27 21:11_

Maybe I should have said "packages read from a registry" instead of "registry packages"?

---

_Review requested from @charliermarsh by @zanieb on 2024-08-27 21:11_

---

_@zanieb reviewed on 2024-08-27 21:11_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:301 on 2024-08-27 21:11_

Is that clearer?

---

_@davidszotten reviewed on 2024-08-28 07:56_

---

_Review comment by @davidszotten on `docs/concepts/resolution.md`:301 on 2024-08-28 07:56_

yes i agree. will update

---

_@davidszotten reviewed on 2024-08-28 07:57_

---

_Review comment by @davidszotten on `docs/concepts/resolution.md`:301 on 2024-08-28 07:57_

or maybe something about "at resolution time" / "when resolving packages" or similar?

---

_@zanieb reviewed on 2024-08-28 12:47_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:301 on 2024-08-28 12:47_

Well, we include the local distributions in the resolution / perform resolution with them — we just don't read them from a registry we read their metadata from the local environment.

---

_@charliermarsh approved on 2024-10-09 23:02_

Thanks, sorry for the delay on this one.

---

_Merged by @charliermarsh on 2024-10-09 23:03_

---

_Closed by @charliermarsh on 2024-10-09 23:03_

---

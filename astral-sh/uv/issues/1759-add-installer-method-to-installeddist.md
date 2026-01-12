```yaml
number: 1759
title: "Add `installer` method to InstalledDist"
type: issue
state: closed
author: tdejager
labels:
  - internal
assignees: []
created_at: 2024-02-20T15:09:18Z
updated_at: 2024-02-20T17:07:10Z
url: https://github.com/astral-sh/uv/issues/1759
synced_at: 2026-01-12T15:58:32Z
```

# Add `installer` method to InstalledDist

---

_@tdejager_

Hey!

I think it would be nice for the `InstalledDist` to be able to acces the `INSTALLER` file directly. This way we could filter based on the Installer type (e.g. uv, conda, pip etc.) when using the `SitePackages` struct for example.

Happy to make a PR for this if you guys think this is a good idea :)

---

_Comment by @charliermarsh on 2024-02-20 15:10_

Go for it! Makes sense to me.

---

_Label `internal` added by @charliermarsh on 2024-02-20 15:10_

---

_Assigned to @tdejager by @charliermarsh on 2024-02-20 15:13_

---

_Comment by @tdejager on 2024-02-20 15:37_

Thanks @charliermarsh. Here it is: https://github.com/astral-sh/uv/pull/1762

---

_Closed by @charliermarsh on 2024-02-20 17:07_

---

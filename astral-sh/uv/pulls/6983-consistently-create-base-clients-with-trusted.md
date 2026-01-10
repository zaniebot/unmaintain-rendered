```yaml
number: 6983
title: Consistently create base clients with trusted host settings
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
draft: true
base: main
head: zb/trusted-host-wip
created_at: 2024-09-03T22:17:02Z
updated_at: 2024-11-07T20:30:00Z
url: https://github.com/astral-sh/uv/pull/6983
synced_at: 2026-01-10T11:59:59Z
```

# Consistently create base clients with trusted host settings

---

_Pull request opened by @zanieb on 2024-09-03 22:17_

Reduces our duplication of base client builders — I'm not sure why we created several different ones in some cases.

Threads some `allowed_insecure_host` configuration around, but we can't actually use it in a couple contexts without moving it from a `ResolverInstallerSetting` to a global option.

---

_Label `enhancement` added by @zanieb on 2024-09-03 22:20_

---

_@charliermarsh approved on 2024-09-04 16:14_

---

_Closed by @zanieb on 2024-11-07 20:30_

---

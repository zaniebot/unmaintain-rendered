```yaml
number: 6985
title: "Support `allow-insecure-host` in `uv python`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-09-03T22:45:56Z
updated_at: 2024-09-07T18:40:05Z
url: https://github.com/astral-sh/uv/issues/6985
synced_at: 2026-01-12T15:59:09Z
```

# Support `allow-insecure-host` in `uv python`

---

_@zanieb_

Currently, we do not read this setting during Python downloads.

See https://github.com/astral-sh/uv/pull/6983 for some beginning changes. We'll need to change the internal structuring of the settings a bit to support this.

---

_Label `enhancement` added by @zanieb on 2024-09-03 22:46_

---

_Comment by @zanieb on 2024-09-03 22:46_

@charliermarsh are you opposed to this / moving the setting to a global option or can I mark this as help wanted?

---

_Comment by @charliermarsh on 2024-09-04 02:21_

@zanieb -- That seems ok to me. Should keyring also be global?

---

_Comment by @zanieb on 2024-09-04 02:56_

@charliermarsh Maybe. I don't see an obvious use-case for it but the network settings make sense for anything that's going to perform requests. Maybe we should move all the network options into a dedicated struct / section?

---

_Comment by @charliermarsh on 2024-09-05 02:01_

@zanieb -- Yeah I support that! (Dedicated struct etc.)

---

_Label `help wanted` added by @zanieb on 2024-09-07 18:40_

---

```yaml
number: 5447
title: "Add support for `install_only_stripped` variants"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-07-25T15:03:39Z
updated_at: 2024-07-25T17:29:32Z
url: https://github.com/astral-sh/uv/issues/5447
synced_at: 2026-01-12T15:58:56Z
```

# Add support for `install_only_stripped` variants

---

_@charliermarsh_

See: https://github.com/indygreg/python-build-standalone/releases/tag/20240725

---

_Label `enhancement` added by @charliermarsh on 2024-07-25 15:03_

---

_Label `help wanted` added by @charliermarsh on 2024-07-25 15:05_

---

_Comment by @zanieb on 2024-07-25 15:07_

We don't have any concept of users requesting specific downloads for each version i.e. we only include a single download per version. However, it seems important for people to be able to acquire builds with debug symbols. Maybe we should

1. Switch over to the stripped distributions by default
2. Create an issue to design a way for users to request installs with debug symbols

---

_Comment by @charliermarsh on 2024-07-25 15:11_

Yeah, that sounds good.

---

_Comment by @charliermarsh on 2024-07-25 15:12_

Do we want users to be able to request specific variants? Or would debug symbols be special (like `--debug` or similar)?

---

_Comment by @zanieb on 2024-07-25 15:13_

I think they'd be special, idk what the use case is for requesting an unoptimized version though. Seems... dubious to allow them all.

---

_Comment by @charliermarsh on 2024-07-25 17:17_

I'll swap over now.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-25 17:18_

---

_Closed by @charliermarsh on 2024-07-25 17:29_

---

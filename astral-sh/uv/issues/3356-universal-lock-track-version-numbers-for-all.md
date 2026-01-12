```yaml
number: 3356
title: "universal-lock: track version numbers for all distributions"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-03T16:01:24Z
updated_at: 2024-05-14T23:07:26Z
url: https://github.com/astral-sh/uv/issues/3356
synced_at: 2026-01-12T15:58:43Z
```

# universal-lock: track version numbers for all distributions

---

_@BurntSushi_

When creating a `Lock`, we currently have a `todo!()` when trying to extract version numbers:

https://github.com/astral-sh/uv/blob/e33ff95575e8165078799548070210947d1cfad0/crates/uv-resolver/src/lock.rs#L281-L287

For example, when the distribution comes from a direct URL, it only has a URL and not a version number:

https://github.com/astral-sh/uv/blob/e33ff95575e8165078799548070210947d1cfad0/crates/distribution-types/src/lib.rs#L567-L571

My understanding is that a version number exists for these distributions in their metadata, but we currently don't keep it around (I believe). We need the version number for our universal lock file, as part of #3347.

---

_Label `preview` added by @BurntSushi on 2024-05-06 12:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-14 19:25_

---

_Closed by @charliermarsh on 2024-05-14 23:07_

---

```yaml
number: 13463
title: Ensure cached realm credentials are applied if no password is found for index URL
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/cached-creds
created_at: 2025-05-15T10:42:14Z
updated_at: 2025-05-15T11:36:19Z
url: https://github.com/astral-sh/uv/pull/13463
synced_at: 2026-01-12T16:10:41Z
```

# Ensure cached realm credentials are applied if no password is found for index URL

---

_@jtfmumm_

We were not correctly falling back to cached realm credentials when an index URL was provided with only a username. This came up in a [later comment](https://github.com/astral-sh/uv/issues/13443#issuecomment-2881115301) on #13443 where credentials in a pip extra index in `uv.toml` were being ignored when the same URL (but with only a username) was used at the command line for `--extra-index-url`. I've added a test to catch this case.

Closes #13443


---

_Label `bug` added by @jtfmumm on 2025-05-15 10:42_

---

_@konstin approved on 2025-05-15 11:35_

---

_Merged by @jtfmumm on 2025-05-15 11:36_

---

_Closed by @jtfmumm on 2025-05-15 11:36_

---

_Branch deleted on 2025-05-15 11:36_

---

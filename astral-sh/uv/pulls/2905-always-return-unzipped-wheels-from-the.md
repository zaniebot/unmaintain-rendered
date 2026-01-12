```yaml
number: 2905
title: Always return unzipped wheels from the distribution database
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/zip
created_at: 2024-04-08T17:40:31Z
updated_at: 2024-04-08T18:07:19Z
url: https://github.com/astral-sh/uv/pull/2905
synced_at: 2026-01-12T16:05:18Z
```

# Always return unzipped wheels from the distribution database

---

_@charliermarsh_

## Summary

In all cases, we unzip these immediately after returning. By moving the unzipping into the database, we can remove a bunch of code (coming in a separate PR), and pave the way for hash-checking, since hash generation will _also_ happen in the database, and splitting the caching layers across the database and the unzipper creates complications.

Closes #2863.


---

_Label `internal` added by @charliermarsh on 2024-04-08 17:40_

---

_Marked ready for review by @charliermarsh on 2024-04-08 17:41_

---

_Converted to draft by @charliermarsh on 2024-04-08 17:48_

---

_Marked ready for review by @charliermarsh on 2024-04-08 18:03_

---

_@zanieb approved on 2024-04-08 18:07_

---

_Merged by @charliermarsh on 2024-04-08 18:07_

---

_Closed by @charliermarsh on 2024-04-08 18:07_

---

_Branch deleted on 2024-04-08 18:07_

---

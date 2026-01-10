```yaml
number: 90
title: "Migrate to `requirements_txt.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2023-10-12T16:47:41Z
updated_at: 2023-10-12T17:09:02Z
url: https://github.com/astral-sh/uv/pull/90
synced_at: 2026-01-10T15:50:28Z
```

# Migrate to `requirements_txt.rs`

---

_Pull request opened by @charliermarsh on 2023-10-12 16:47_

Remove the parser I wrote in favor of Konsti's which is much more complete. The only change vs. the version in `poc-monotrail` is that I changed the tests to use insta rather than manually storing and comparing against JSON snapshots.

Closes https://github.com/astral-sh/puffin/issues/89.

---

_Merged by @charliermarsh on 2023-10-12 17:09_

---

_Closed by @charliermarsh on 2023-10-12 17:09_

---

_Branch deleted on 2023-10-12 17:09_

---

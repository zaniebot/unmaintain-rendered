```yaml
number: 2592
title: Fix authentication with JFrog artifactories
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-jfrog
created_at: 2024-03-21T16:22:12Z
updated_at: 2024-03-21T17:10:44Z
url: https://github.com/astral-sh/uv/pull/2592
synced_at: 2026-01-10T14:49:08Z
```

# Fix authentication with JFrog artifactories

---

_Pull request opened by @zanieb on 2024-03-21 16:22_

Closes #2566 

We were storing the username e.g. `charlie@astral.sh` as a percent-encoded string `charlie%40astral.sh` which resulted in different headers and broke JFrog's artifactory which apparently does not decode usernames.

Tested with a JFrog artifactory and AWS CodeArtifact although it is worth noting that AWS does _not_ have a username with an `@` — it'd be nice to test another artifactory with percent-encoded characters in the username and/or password.

---

_Label `bug` added by @zanieb on 2024-03-21 16:22_

---

_@charliermarsh approved on 2024-03-21 17:08_

---

_Merged by @zanieb on 2024-03-21 17:10_

---

_Closed by @zanieb on 2024-03-21 17:10_

---

_Branch deleted on 2024-03-21 17:10_

---

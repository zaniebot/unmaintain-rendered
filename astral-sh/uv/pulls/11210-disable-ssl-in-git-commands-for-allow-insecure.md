```yaml
number: 11210
title: "Disable SSL in Git commands for `--allow-insecure-host`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/allow
created_at: 2025-02-04T03:19:04Z
updated_at: 2025-02-05T04:26:32Z
url: https://github.com/astral-sh/uv/pull/11210
synced_at: 2026-01-12T16:09:44Z
```

# Disable SSL in Git commands for `--allow-insecure-host`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/11176.

## Test Plan

- Created a self-signed certificate.
- Ran `openssl s_server -cert cert.pem -key key.pem -WWW -port 8443`.
- Verified that `cargo run pip install git+https://localhost:8443/repo.git` failed with:

```
error: Git operation failed
  Caused by: failed to fetch into: /Users/crmarsh/.cache/uv/git-v0/db/0773914b3ec4a56e
  Caused by: process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://localhost:8443/repo.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
--- stderr
fatal: unable to access 'https://localhost:8443/repo.git/': SSL certificate problem: self signed certificate
```

- Verified that `cargo run pip install git+https://localhost:8443/repo.git --allow-insecure-host https://localhost:8443` continued further.


---

_Label `bug` added by @charliermarsh on 2025-02-04 03:19_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-04 03:19_

---

_@konstin approved on 2025-02-04 11:18_

---

_Merged by @charliermarsh on 2025-02-04 15:57_

---

_Closed by @charliermarsh on 2025-02-04 15:57_

---

_Branch deleted on 2025-02-04 15:58_

---

_Comment by @zanieb on 2025-02-05 04:26_

Nice work!

---

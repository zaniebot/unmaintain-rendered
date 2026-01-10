```yaml
number: 8703
title: "Run `cargo update`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/async-
created_at: 2024-10-30T17:19:45Z
updated_at: 2024-11-01T14:26:32Z
url: https://github.com/astral-sh/uv/pull/8703
synced_at: 2026-01-10T11:59:59Z
```

# Run `cargo update`

---

_Pull request opened by @charliermarsh on 2024-10-30 17:19_

Pull in https://github.com/prefix-dev/async_http_range_reader/pull/19, removes a version of `itertools`.

---

_Label `internal` added by @charliermarsh on 2024-10-30 17:19_

---

_@zanieb approved on 2024-10-30 17:32_

---

_Comment by @charliermarsh on 2024-10-30 18:20_

Looks like something changed in zlib-ng-sys that's causing Windows to fail. Haven't looked into it yet.

---

_Comment by @konstin on 2024-10-31 10:28_

Reported upstream: https://github.com/rust-lang/libz-sys/issues/227

---

_Comment by @charliermarsh on 2024-10-31 13:00_

Thanks, I had reported it here: https://github.com/rust-lang/libz-sys/issues/225#issuecomment-2448008837

---

_Merged by @charliermarsh on 2024-11-01 14:26_

---

_Closed by @charliermarsh on 2024-11-01 14:26_

---

_Branch deleted on 2024-11-01 14:26_

---

_Comment by @charliermarsh on 2024-11-01 14:26_

I pinned it for now with a link to the issue.

---

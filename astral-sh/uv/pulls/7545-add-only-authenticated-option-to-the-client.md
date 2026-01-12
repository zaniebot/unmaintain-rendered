```yaml
number: 7545
title: "Add `only_authenticated` option to the client"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/only_authenticated
created_at: 2024-09-19T11:52:11Z
updated_at: 2024-09-21T14:09:16Z
url: https://github.com/astral-sh/uv/pull/7545
synced_at: 2026-01-12T16:07:53Z
```

# Add `only_authenticated` option to the client

---

_@konstin_

When sending an upload request, we use an HTTP formdata request, which can't be cloned (https://github.com/seanmonstar/reqwest/issues/2416, plus a limitation that formdata bodies are always internally streaming), but we also know that we need to always have credentials.

The authentication middleware by default tries to clone the request and send an authenticated request first. By introducing an `only_authenticated` setting, we can skip this behaviour for publishing.

The second change i rolled in here is not adding the retry middleware when retries are 0, since the retry middleware would always clone the request (it needs to store a copy in case the request fails).

I'm open to switching to any different workaround for the cloneable-request problem.

Split out from #7475


---

_Label `internal` added by @konstin on 2024-09-19 11:52_

---

_Review requested from @zanieb by @konstin on 2024-09-19 11:52_

---

_@zanieb approved on 2024-09-21 14:00_

---

_Comment by @zanieb on 2024-09-21 14:01_

I might change this in the future to do something like pass a list of URLs which we should not perform unauthenticated requests to — we need that for https://github.com/astral-sh/uv/issues/4583. This looks fine in the meantime though unless you want to chase that.

---

_Merged by @konstin on 2024-09-21 14:09_

---

_Closed by @konstin on 2024-09-21 14:09_

---

_Branch deleted on 2024-09-21 14:09_

---

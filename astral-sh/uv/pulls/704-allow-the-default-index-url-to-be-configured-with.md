```yaml
number: 704
title: "Allow the default index url to be configured with `PUFFIN_INDEX_URL`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/env-index
created_at: 2023-12-19T17:40:53Z
updated_at: 2023-12-20T10:52:01Z
url: https://github.com/astral-sh/uv/pull/704
synced_at: 2026-01-12T16:04:08Z
```

# Allow the default index url to be configured with `PUFFIN_INDEX_URL`

---

_@zanieb_

This allows the default index URL to be easily overridden with a local index e.g. a `packse` server

```
export PUFFIN_INDEX_URL="http://localhost:3141/packages/all/+simple"
```

---

_Comment by @zanieb on 2023-12-19 22:24_

Requires ca8d519ae4ac7ab31f8251d09ced72fbbe2d3d2b for some tests, we might want to consider loading the environment variable somewhere more internal?

---

_@charliermarsh approved on 2023-12-20 02:42_

---

_@konstin approved on 2023-12-20 10:51_

---

_Merged by @konstin on 2023-12-20 10:52_

---

_Closed by @konstin on 2023-12-20 10:52_

---

_Branch deleted on 2023-12-20 10:52_

---

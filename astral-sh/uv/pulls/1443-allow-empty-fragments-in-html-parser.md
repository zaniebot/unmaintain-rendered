```yaml
number: 1443
title: Allow empty fragments in HTML parser
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty-hash
created_at: 2024-02-16T06:38:53Z
updated_at: 2024-02-16T06:42:22Z
url: https://github.com/astral-sh/uv/pull/1443
synced_at: 2026-01-10T15:33:24Z
```

# Allow empty fragments in HTML parser

---

_Pull request opened by @charliermarsh on 2024-02-16 06:38_

## Summary

It looks like `devpi` might add an empty fragment (`#`) at the end of the URL. We expect it to contain the hash; this just makes empty-fragment map to "no hash".

Closes https://github.com/astral-sh/uv/issues/1441.


---

_Label `bug` added by @charliermarsh on 2024-02-16 06:41_

---

_Merged by @charliermarsh on 2024-02-16 06:42_

---

_Closed by @charliermarsh on 2024-02-16 06:42_

---

_Branch deleted on 2024-02-16 06:42_

---

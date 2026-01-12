```yaml
number: 6133
title: Parse registry URLs lazily
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-08-16T01:08:47Z
updated_at: 2025-01-01T17:35:31Z
url: https://github.com/astral-sh/uv/issues/6133
synced_at: 2026-01-12T15:59:01Z
```

# Parse registry URLs lazily

---

_@charliermarsh_

Right now, we parse the URL in `File` to pass a `UrlString` to `FileLocation`. I think we should consider just storing the string, and parsing the URL when we try to fetch the file? The vast majority of `File` that we deserialize never get requested.


---

_Label `performance` added by @charliermarsh on 2024-08-16 01:08_

---

_Closed by @charliermarsh on 2025-01-01 17:35_

---

_Closed by @charliermarsh on 2025-01-01 17:35_

---

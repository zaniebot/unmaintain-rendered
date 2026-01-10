```yaml
number: 568
title: Avoid storing zipped wheels in the cache
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-12-05T18:15:05Z
updated_at: 2023-12-06T04:02:08Z
url: https://github.com/astral-sh/uv/issues/568
synced_at: 2026-01-10T05:40:31Z
```

# Avoid storing zipped wheels in the cache

---

_Issue opened by @charliermarsh on 2023-12-05 18:15_

Every cache location should be "unique".

---

_Label `internal` added by @charliermarsh on 2023-12-05 18:15_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-05 18:17_

---

_Comment by @charliermarsh on 2023-12-06 04:02_

We've accomplished the primary goal here of ensuring that zipped and unzipped wheels are written to unique locations. Whether we retain or remove the zipped wheels is a separate consideration.

---

_Closed by @charliermarsh on 2023-12-06 04:02_

---

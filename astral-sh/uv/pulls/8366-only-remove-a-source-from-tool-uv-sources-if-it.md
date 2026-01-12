```yaml
number: 8366
title: "Only remove a source from `[tool.uv.sources]` if it is no long being referenced"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: remove-source
created_at: 2024-10-19T14:15:47Z
updated_at: 2024-10-21T23:20:44Z
url: https://github.com/astral-sh/uv/pull/8366
synced_at: 2026-01-12T16:08:17Z
```

# Only remove a source from `[tool.uv.sources]` if it is no long being referenced

---

_@j178_

## Summary

Resolves #8361

---

_Renamed from "Only remove `[tool.uv.sources]` if it is no long being referenced" to "Only remove a source from `[tool.uv.sources]` if it is no long being referenced" by @j178 on 2024-10-19 14:17_

---

_Comment by @zanieb on 2024-10-19 14:35_

I finally got #8360 past the flaky CI which should cover this case.

---

_Marked ready for review by @j178 on 2024-10-19 15:11_

---

_Converted to draft by @j178 on 2024-10-19 15:31_

---

_Marked ready for review by @j178 on 2024-10-19 15:32_

---

_@zanieb approved on 2024-10-19 15:51_

---

_Comment by @zanieb on 2024-10-19 15:51_

Thanks again!

---

_Merged by @zanieb on 2024-10-19 15:52_

---

_Closed by @zanieb on 2024-10-19 15:52_

---

_Branch deleted on 2024-10-19 16:09_

---

_Comment by @zanieb on 2024-10-21 22:13_

Awkwardly, sources can be defined for transitive dependencies so we might need to check that it's not present in the resolution?

https://github.com/astral-sh/uv/issues/7454#issuecomment-2355710565

Relatively niche, but someone will notice eventually.

cc @charliermarsh 

---

_Comment by @charliermarsh on 2024-10-21 23:20_

I believe sources are only respected for dependencies defined in the workspace, not arbitrary transitive dependencies.

---

```yaml
number: 7645
title: "Show a dedicated PubGrub hint for `--unsafe-best-match`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/index-error
created_at: 2024-09-23T16:40:27Z
updated_at: 2024-09-23T19:02:20Z
url: https://github.com/astral-sh/uv/pull/7645
synced_at: 2026-01-12T16:07:55Z
```

# Show a dedicated PubGrub hint for `--unsafe-best-match`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5510.


---

_Review requested from @zanieb by @charliermarsh on 2024-09-23 16:40_

---

_Review requested from @konstin by @charliermarsh on 2024-09-23 16:40_

---

_@charliermarsh reviewed on 2024-09-23 16:41_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:12413 on 2024-09-23 16:41_

I'm not sure if we want to include more info here on what our default behavior is and why. I think "If both indexes are equally trusted" and the name `unsafe-best-match` imply that it's for security reasons, so I left it as-is.

---

_@zanieb reviewed on 2024-09-23 18:02_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:12413 on 2024-09-23 18:02_

I think we should provide a bit more information here. Verbose is better than not in these hints.

We never link to the documentation from the CLI right now, but we might want to do that in the future.

---

_@zanieb approved on 2024-09-23 18:03_

---

_Merged by @charliermarsh on 2024-09-23 19:02_

---

_Closed by @charliermarsh on 2024-09-23 19:02_

---

_Branch deleted on 2024-09-23 19:02_

---

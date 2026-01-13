```yaml
number: 17412
title: "Skip uploads when validation reports 'Already uploaded'"
type: pull_request
state: open
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/val
created_at: 2026-01-12T04:09:40Z
updated_at: 2026-01-13T08:02:21Z
url: https://github.com/astral-sh/uv/pull/17412
synced_at: 2026-01-13T08:23:24Z
```

# Skip uploads when validation reports 'Already uploaded'

---

_@charliermarsh_

## Summary

If the file was already uploaded, `/validate` already tells us that.


---

_Review requested from @konstin by @charliermarsh on 2026-01-12 04:16_

---

_@zanieb approved on 2026-01-12 23:58_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:660 on 2026-01-13 08:02_

Can we use something typed like JSON for this instead of parsing plain text bodies?

---

_@konstin reviewed on 2026-01-13 08:02_

---

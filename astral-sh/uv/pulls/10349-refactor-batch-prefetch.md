```yaml
number: 10349
title: Refactor batch prefetch
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/split-batch-prefetch
created_at: 2025-01-07T09:40:41Z
updated_at: 2025-01-07T13:58:47Z
url: https://github.com/astral-sh/uv/pull/10349
synced_at: 2026-01-12T16:09:15Z
```

# Refactor batch prefetch

---

_@konstin_

Ref https://github.com/astral-sh/uv/issues/10344

The first commit split determining and executing batch prefetch. The second commit splits out a batch prefetch runner type. It comes from a failed attempt to use threading for the batch prefetch, separating the concerns between deciding whether to prefetch (resolver thread) and executing the prefetch (potentially expensive, possible to move to another thread).

No functional changes.

I have confirmed that urllib3/boto3/botocore is still fast (`uv pip compile --no-cache-dir scripts/requirements/boto3.in` gives `Resolved 7 packages in 1.21s`)

---

_Label `internal` added by @konstin on 2025-01-07 09:40_

---

_Review requested from @charliermarsh by @konstin on 2025-01-07 09:40_

---

_@konstin reviewed on 2025-01-07 09:41_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:328 on 2025-01-07 09:41_

Should this live somewhere shared?

---

_@charliermarsh approved on 2025-01-07 13:53_

---

_@charliermarsh reviewed on 2025-01-07 13:53_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:328 on 2025-01-07 13:53_

I think it's alright for now.

---

_Merged by @konstin on 2025-01-07 13:58_

---

_Closed by @konstin on 2025-01-07 13:58_

---

_Branch deleted on 2025-01-07 13:58_

---

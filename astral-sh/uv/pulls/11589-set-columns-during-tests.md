```yaml
number: 11589
title: "Set `COLUMNS` during tests "
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: ci-columns
created_at: 2025-02-18T07:21:03Z
updated_at: 2025-02-26T16:10:36Z
url: https://github.com/astral-sh/uv/pull/11589
synced_at: 2026-01-10T11:10:38Z
```

# Set `COLUMNS` during tests 

---

_Pull request opened by @mgorny on 2025-02-18 07:21_

## Summary

When tests are run downstream, the `COLUMNS` environment variable is used to force fixed output width and avoid test failures due to different terminal widths.  However, this occasionally causes test regressions when other tests rely on different output width. Use the same `COLUMNS` value in CI to ensure consistent output and catch any regressions.

## Test Plan

It wasn't, it's supposed to be tested by the CI :-).


---

_Comment by @mgorny on 2025-02-18 07:31_

It failed with #11588, so it works :-).

---

_Review requested from @zanieb by @konstin on 2025-02-18 08:41_

---

_Label `internal` added by @konstin on 2025-02-18 08:41_

---

_Comment by @zanieb on 2025-02-18 14:37_

Thank you!

Can we set this in the test context instead?

https://github.com/astral-sh/uv/blob/5765e4bdee26fae7208d9fc380568bde2db66712/crates/uv/tests/it/common/mod.rs#L565

We may want a comment about how we already disable line-wrapping in all of _our_ output for snapshots but snapshotted output from other tools may still wrap?

---

_@zanieb approved on 2025-02-18 15:52_

---

_Renamed from "Set `COLUMNS` in Linux CI workflow to prevent regressions" to "Set `COLUMNS` during tests " by @zanieb on 2025-02-18 15:54_

---

_Comment by @mgorny on 2025-02-18 16:31_

Sorry for not handling the CI problems timely, and thanks for taking it over!

---

_Merged by @zanieb on 2025-02-18 16:38_

---

_Closed by @zanieb on 2025-02-18 16:38_

---

_Branch deleted on 2025-02-20 04:10_

---

_Comment by @zanieb on 2025-02-26 16:10_

No problem!

---

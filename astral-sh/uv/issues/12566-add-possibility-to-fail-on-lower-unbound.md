---
number: 12566
title: "Add possibility to fail on lower-unbound dependency with --resolution lowest*"
type: issue
state: open
author: potiuk
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-03-30T23:53:57Z
updated_at: 2025-04-01T23:27:16Z
url: https://github.com/astral-sh/uv/issues/12566
synced_at: 2026-01-10T01:25:21Z
---

# Add possibility to fail on lower-unbound dependency with --resolution lowest*

---

_Issue opened by @potiuk on 2025-03-30 23:53_

### Summary

After fully switching to workspaces in Airflow, we are using `uv sync --resolution lowest-direct` to check if our 90+ providers have properly set lower-bounds. This works really nicely now after refactoring some of the code and ti will work even better after https://github.com/apache/airflow/pull/48223 where we complete the refactoring and switch to `uv` completely.

While runnign the tests I have found however that in a few cases we accidentally missed to put lower bounds in some of our dependencies - in both "production" and "development dependencies and we got the warnings:

```
warning: The direct dependency `microsoft-kiota-abstractions` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
warning: The direct dependency `pgvector` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
warning: The direct dependency `rich` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
warning: The direct dependency `ruff` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
```

I would like to force failure in such case and treat lack for lower-binding as a failure.

Would it be possible to add a flag to switch the warning into e failure?


### Example

_No response_

---

_Label `enhancement` added by @potiuk on 2025-03-30 23:53_

---

_Comment by @zanieb on 2025-04-01 21:31_

Seems reasonable to me, though I'm not quite sure how we'd spell it? `lowest-strict`?

---

_Label `needs-design` added by @zanieb on 2025-04-01 21:32_

---

_Comment by @potiuk on 2025-04-01 23:26_

Maybe additional flag `--fail-on-unbounded` ?

---

_Comment by @potiuk on 2025-04-01 23:27_

or smth :)

---

```yaml
number: 17471
title: Reduce the number of CI checks run on pull requests
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
base: main
head: zb/investigate-ci
created_at: 2026-01-14T17:22:43Z
updated_at: 2026-01-14T17:27:26Z
url: https://github.com/astral-sh/uv/pull/17471
synced_at: 2026-01-14T17:38:04Z
```

# Reduce the number of CI checks run on pull requests

---

_@zanieb_

We're hitting GitHub concurrency limits (organization wide limit of 60 jobs), and while we could move to paid runners with high concurrency limits, I'd prefer to stay on the free runners and some of these jobs, e.g., `test-system`, require GitHub runners.

This moves a bunch of our extended testing behind a label, e.g., `test:extended` or `test:system`, and only runs them on `main` by default.

---

_Renamed from "zb/investigate ci" to "Reduce the number of CI checks run on pull requests" by @zanieb on 2026-01-14 17:23_

---

_@zanieb reviewed on 2026-01-14 17:27_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:20 on 2026-01-14 17:27_

I'm going to refactor all these checks in a follow-up

---

_Marked ready for review by @zanieb on 2026-01-14 17:27_

---

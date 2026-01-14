```yaml
number: 17471
title: Reduce the number of CI checks run on pull requests
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/investigate-ci
created_at: 2026-01-14T17:22:43Z
updated_at: 2026-01-14T19:12:14Z
url: https://github.com/astral-sh/uv/pull/17471
synced_at: 2026-01-14T19:46:16Z
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

_Label `internal` added by @zanieb on 2026-01-14 17:42_

---

_@zsol approved on 2026-01-14 18:15_

I think we should run the smoke tests on all changes. They seem to be quick enough by reading them (I can't find any historical data for them easily)

Otherwise LGTM

---

_Comment by @zanieb on 2026-01-14 18:33_

Here's a nice Claude summary

```

  Job Runtimes by Workflow (from recent main branch run)
  ┌──────────────────┬──────┬────────────┬─────────┐
  │     Workflow     │ Jobs │ Total Time │ Avg/Job │
  ├──────────────────┼──────┼────────────┼─────────┤
  │ test-system      │ 30   │ 32m 58s    │ 1m 05s  │
  ├──────────────────┼──────┼────────────┼─────────┤
  │ test-integration │ 20   │ 15m 33s    │ 0m 46s  │
  ├──────────────────┼──────┼────────────┼─────────┤
  │ test-smoke       │ 6    │ 4m 18s     │ 0m 43s  │
  ├──────────────────┼──────┼────────────┼─────────┤
  │ test-ecosystem   │ 3    │ 1m 10s     │ 0m 23s  │
  └──────────────────┴──────┴────────────┴─────────┘
  Breakdown by Runner Type
  ┌──────────────────┬─────────┬──────┬────────────┐
  │     Workflow     │ Runner  │ Jobs │ Total Time │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-system      │ linux   │ 16   │ 16m 16s    │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-system      │ windows │ 8    │ 12m 07s    │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-system      │ macos   │ 6    │ 4m 35s     │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-integration │ linux   │ 12   │ 10m 33s    │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-integration │ windows │ 7    │ 4m 47s     │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-integration │ macos   │ 1    │ 0m 13s     │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-smoke       │ windows │ 2    │ 1m 40s     │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-smoke       │ macos   │ 1    │ 1m 28s     │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-smoke       │ linux   │ 3    │ 1m 10s     │
  ├──────────────────┼─────────┼──────┼────────────┤
  │ test-ecosystem   │ linux   │ 3    │ 1m 10s     │
  └──────────────────┴─────────┴──────┴────────────┘
  Longest Running Jobs
  ┌───────────────────────────────────────────┬──────────┐
  │                    Job                    │ Duration │
  ├───────────────────────────────────────────┼──────────┤
  │ test-system / pyston on linux             │ 3m 54s   │
  ├───────────────────────────────────────────┼──────────┤
  │ test-system / conda3.11 on windows x86-64 │ 3m 12s   │
  ├───────────────────────────────────────────┼──────────┤
  │ test-system / conda3.8 on windows x86-64  │ 2m 46s   │
  ├───────────────────────────────────────────┼──────────┤
  │ test-system / python3.10 on windows x86   │ 2m 25s   │
  ├───────────────────────────────────────────┼──────────┤
  │ test-integration / uv_build               │ 2m 17s   │
  ├───────────────────────────────────────────┼──────────┤
  │ test-integration / pyenv on wsl           │ 2m 07s   │
  └───────────────────────────────────────────┴──────────┘
  ```

---

_Merged by @zanieb on 2026-01-14 19:12_

---

_Closed by @zanieb on 2026-01-14 19:12_

---

_Branch deleted on 2026-01-14 19:12_

---

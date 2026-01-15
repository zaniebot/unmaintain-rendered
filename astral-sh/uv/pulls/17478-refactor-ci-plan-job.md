```yaml
number: 17478
title: Refactor CI plan job
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: claude/refactor-ci-plan-job-CA3KE
created_at: 2026-01-14T22:50:52Z
updated_at: 2026-01-15T17:15:55Z
url: https://github.com/astral-sh/uv/pull/17478
synced_at: 2026-01-15T17:50:37Z
```

# Refactor CI plan job

---

_@zanieb_

Aiming for more readability and maintainability here. Sort of limited by bash and GitHub Actions, but I don't think it quite merits a change to Python yet.

---

_Label `internal` added by @zanieb on 2026-01-14 22:50_

---

_@github-advanced-security[bot] reviewed on 2026-01-14 23:22_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/ci.yml`:41 on 2026-01-14 23:22_

code injection via template expansion

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/230)

---

_Marked ready for review by @zanieb on 2026-01-15 05:43_

---

_Comment by @zanieb on 2026-01-15 05:44_

I'm not in love with where I landed here but it does seem like an improvement?


---

_Merged by @zanieb on 2026-01-15 17:15_

---

_Closed by @zanieb on 2026-01-15 17:15_

---

_Comment by @zanieb on 2026-01-15 17:15_

I want to iterate on this, please feel free to review afterwards.

---

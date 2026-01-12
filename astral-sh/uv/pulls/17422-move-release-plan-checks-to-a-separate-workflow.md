```yaml
number: 17422
title: Move release plan checks to a separate workflow
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/release-plan
created_at: 2026-01-12T19:15:26Z
updated_at: 2026-01-12T20:49:43Z
url: https://github.com/astral-sh/uv/pull/17422
synced_at: 2026-01-12T21:26:06Z
```

# Move release plan checks to a separate workflow

---

_@zanieb_

We get a bunch of redundant skipped `Release / Build binary ...` jobs in CI otherwise, and I would rather the release workflow didn't have a pull request trigger at all.

Follows #17388 

---

_@github-advanced-security[bot] reviewed on 2026-01-12 19:16_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/check-release-plan.yml`:12 on 2026-01-12 19:16_

detects commit SHAs that don't match their version comment tags

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/227)

---

_@Gankra approved on 2026-01-12 20:46_

---

_Marked ready for review by @zanieb on 2026-01-12 20:49_

---

_Merged by @zanieb on 2026-01-12 20:49_

---

_Closed by @zanieb on 2026-01-12 20:49_

---

_Branch deleted on 2026-01-12 20:49_

---

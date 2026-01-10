```yaml
number: 17388
title: "Split up `ci.yml`"
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
base: main
head: zb/reusable-workflow
created_at: 2026-01-09T19:43:46Z
updated_at: 2026-01-10T00:22:58Z
url: https://github.com/astral-sh/uv/pull/17388
synced_at: 2026-01-10T05:49:14Z
```

# Split up `ci.yml`

---

_Pull request opened by @zanieb on 2026-01-09 19:43_

This file is too big for an LLM context window and several contributors have complained about it being too scary to touch.

This also gets us collapsible sections in the UI.

I renamed some jobs for clarity in the meantime. And added a meta-job for required checks passing so we can avoid churn in our "Settings" when we change job names.

Note this was entirely refactored by Claude.

---

_@github-advanced-security[bot] reviewed on 2026-01-09 19:44_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/release.yml`:98 on 2026-01-09 19:44_

secrets unconditionally inherited by called workflow

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/195)

---

_Marked ready for review by @zanieb on 2026-01-09 22:05_

---

_@zanieb reviewed on 2026-01-10 00:22_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:98 on 2026-01-10 00:22_

This is pre-existing.

---

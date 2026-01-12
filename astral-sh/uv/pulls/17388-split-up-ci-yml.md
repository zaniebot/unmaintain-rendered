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
updated_at: 2026-01-12T15:28:46Z
url: https://github.com/astral-sh/uv/pull/17388
synced_at: 2026-01-12T16:12:45Z
```

# Split up `ci.yml`

---

_@zanieb_

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

_@charliermarsh reviewed on 2026-01-12 15:25_

---

_Review comment by @charliermarsh on `.github/workflows/publish-docs.yml`:39 on 2026-01-12 15:25_

May want to decouple this?

---

_@charliermarsh approved on 2026-01-12 15:25_

---

_@zanieb reviewed on 2026-01-12 15:28_

---

_Review comment by @zanieb on `.github/workflows/publish-docs.yml`:39 on 2026-01-12 15:28_

We were using inconsistent versions across the workflows. I'm not even sure why. I did have Claude audit that we weren't bumping to a version that we weren't using elsewhere in the file. I'll double check though.

---

```yaml
number: 57
title: Basic CI workflow
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: delete-unused-release-jobs
head: micha/ci
created_at: 2025-05-06T09:04:41Z
updated_at: 2025-07-08T10:38:22Z
url: https://github.com/astral-sh/ty/pull/57
synced_at: 2026-01-10T02:34:10Z
```

# Basic CI workflow

---

_Pull request opened by @MichaReiser on 2025-05-06 09:04_

_No description provided._

---

_Marked ready for review by @MichaReiser on 2025-05-06 09:30_

---

_Review requested from @zanieb by @MichaReiser on 2025-05-06 09:30_

---

_Label `ci` added by @MichaReiser on 2025-05-06 09:38_

---

_@MichaReiser reviewed on 2025-05-06 09:38_

---

_Review comment by @MichaReiser on `.github/workflows/build-docker.yml`:24 on 2025-05-06 09:38_

@zanieb does this look correct to you for the docker build job

---

_@zanieb reviewed on 2025-05-06 12:16_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:24 on 2025-05-06 12:16_

Why is it changing? Are you just adding permissions? This will probably need like `packages: read` too?

---

_@zanieb reviewed on 2025-05-06 12:20_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:24 on 2025-05-06 12:20_

Note we need `packages: write` for some jobs here

There are also permissions defined at

https://github.com/astral-sh/ty/blob/aeff25fea689632d0a6c2fce0aea27fd2c188d98/dist-workspace.toml#L67

---

_@MichaReiser reviewed on 2025-05-06 12:21_

---

_Review comment by @MichaReiser on `.github/workflows/build-docker.yml`:24 on 2025-05-06 12:21_

linters are complaining about allowing all permissions. I see that there's already a `packages: "write"` further down.

---

_@MichaReiser reviewed on 2025-05-06 12:53_

---

_Review comment by @MichaReiser on `.github/workflows/build-docker.yml`:24 on 2025-05-06 12:53_

Is there a way for me to verify that the job still runs successfully?

---

_@zanieb reviewed on 2025-05-06 13:05_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:24 on 2025-05-06 13:05_

I don't think so, the build is tested but the rest can't be tested without a release.

---

_@zanieb approved on 2025-05-06 13:06_

---

_Merged by @zanieb on 2025-05-06 13:18_

---

_Closed by @zanieb on 2025-05-06 13:18_

---

_Branch deleted on 2025-07-08 10:38_

---

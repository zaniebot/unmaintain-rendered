```yaml
number: 16439
title: Pin the maturin version in the release pipeline
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/maturin-pin
created_at: 2025-10-24T15:51:11Z
updated_at: 2025-10-24T18:16:33Z
url: https://github.com/astral-sh/uv/pull/16439
synced_at: 2026-01-10T06:36:16Z
```

# Pin the maturin version in the release pipeline

---

_Pull request opened by @zanieb on 2025-10-24 15:51_

This should avoid hitting the GitHub API to determine the latest release, and more generally we should not automatically fetch the latest version of Maturin in our release pipeline as it opens us to supply-chain attacks.

---

_Label `internal` added by @zanieb on 2025-10-24 15:51_

---

_Marked ready for review by @zanieb on 2025-10-24 15:56_

---

_Review requested from @konstin by @zanieb on 2025-10-24 15:56_

---

_Review requested from @woodruffw by @zanieb on 2025-10-24 15:56_

---

_@woodruffw approved on 2025-10-24 18:11_

LGTM -- I *think* there's a way to configure Renovate with a checker that can periodically update this too, but I haven't looked super closely into it yet.

---

_Merged by @zanieb on 2025-10-24 18:16_

---

_Closed by @zanieb on 2025-10-24 18:16_

---

_Branch deleted on 2025-10-24 18:16_

---

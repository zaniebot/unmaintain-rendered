```yaml
number: 15175
title: "workflows: Add annotating permission to 'annotate uv' action"
type: pull_request
state: open
author: geofft
labels:
  - no-build
  - no-test
assignees: []
base: main
head: geofft/fix-annotate-permissions
created_at: 2025-08-08T20:11:23Z
updated_at: 2025-08-08T20:27:22Z
url: https://github.com/astral-sh/uv/pull/15175
synced_at: 2026-01-12T16:11:37Z
```

# workflows: Add annotating permission to 'annotate uv' action

---

_@geofft_

_No description provided._

---

_Review requested from @zanieb by @geofft on 2025-08-08 20:11_

---

_Review requested from @woodruffw by @geofft on 2025-08-08 20:11_

---

_Label `no-build` added by @geofft on 2025-08-08 20:11_

---

_Label `no-test` added by @geofft on 2025-08-08 20:11_

---

_Comment by @geofft on 2025-08-08 20:15_

Er, annotations vs. attestations, oops. This probably needs `packages: write`, and perhaps `id-token: write`.

---

_Comment by @woodruffw on 2025-08-08 20:27_

> Er, annotations vs. attestations, oops. This probably needs `packages: write`, and perhaps `id-token: write`.

Yeah, `attestations` needs `id-token` so you'll need that for sure 🙂 

(I'm very confused by when `packages: write` is needed with GHCR. I guess maybe it's needed here because we're modifying the manifest with `docker buildx ...`?)

---

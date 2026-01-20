```yaml
number: 17542
title: Add a nextest setup hook and optional code signing for tests on macOS
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/sign-macos
created_at: 2026-01-16T22:18:26Z
updated_at: 2026-01-20T14:43:43Z
url: https://github.com/astral-sh/uv/pull/17542
synced_at: 2026-01-20T15:44:17Z
```

# Add a nextest setup hook and optional code signing for tests on macOS

---

_@zanieb_

Uses a nextest setup hook to sign the uv and test binaries before running the tests. This allows you to grant permission to the test suite _once_ when running native authentication tests on macOS. Otherwise, you get prompted on every access on every binary change.

---

_Label `internal` added by @zanieb on 2026-01-16 22:18_

---

_Label `testing` added by @zanieb on 2026-01-16 22:18_

---

_Review requested from @woodruffw by @zanieb on 2026-01-17 16:23_

---

_Marked ready for review by @zanieb on 2026-01-17 16:43_

---

_Review comment by @woodruffw on `scripts/codesign-macos.sh`:34 on 2026-01-19 19:41_

I might be wrong about this, but I *think* we could use the ad-hoc identity here (maybe as a fallback?) so that external contributors could also run these tests (also so that every Astral member doesn't need access to the real signing cert.)

For that, I think `--sign -` would do the trick. 

---

_Review comment by @woodruffw on `scripts/codesign-macos.sh`:34 on 2026-01-19 19:42_

(Or I guess the user can just set that via `UV_CODESIGN_IDENTITY`)

---

_@woodruffw approved on 2026-01-19 19:42_

---

_Review comment by @zanieb on `scripts/codesign-macos.sh`:34 on 2026-01-20 14:43_

I don't think it works, per our discussion in Discord.

---

_@zanieb reviewed on 2026-01-20 14:43_

---

_Merged by @zanieb on 2026-01-20 14:43_

---

_Closed by @zanieb on 2026-01-20 14:43_

---

_Branch deleted on 2026-01-20 14:43_

---

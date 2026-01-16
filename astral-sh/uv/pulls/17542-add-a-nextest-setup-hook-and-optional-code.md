```yaml
number: 17542
title: Add a nextest setup hook and optional code signing for tests on macOS
type: pull_request
state: open
author: zanieb
labels:
  - internal
  - testing
assignees: []
draft: true
base: main
head: zb/sign-macos
created_at: 2026-01-16T22:18:26Z
updated_at: 2026-01-16T22:38:03Z
url: https://github.com/astral-sh/uv/pull/17542
synced_at: 2026-01-16T23:06:08Z
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

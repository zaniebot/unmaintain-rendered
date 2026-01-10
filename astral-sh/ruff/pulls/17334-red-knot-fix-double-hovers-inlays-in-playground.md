```yaml
number: 17334
title: "[red-knot] Fix double hovers/inlays in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/fix-double-hovers
created_at: 2025-04-10T12:37:29Z
updated_at: 2025-04-10T12:40:42Z
url: https://github.com/astral-sh/ruff/pull/17334
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Fix double hovers/inlays in playground

---

_Pull request opened by @MichaReiser on 2025-04-10 12:37_

## Summary

I added a `key` to `Monaco` which means that `onMount` can now run more than once (in fact, it runs once on first initialization and then once everytime the user hits reset). 

This PR fixes the double (or triple!) hover and inlays by disposing the old server when `onMount` is executed again

Fixes https://github.com/astral-sh/ruff/issues/17329, https://github.com/astral-sh/ruff/issues/17330




---

_Label `playground` added by @MichaReiser on 2025-04-10 12:37_

---

_Label `red-knot` added by @MichaReiser on 2025-04-10 12:37_

---

_Merged by @MichaReiser on 2025-04-10 12:40_

---

_Closed by @MichaReiser on 2025-04-10 12:40_

---

_Branch deleted on 2025-04-10 12:40_

---

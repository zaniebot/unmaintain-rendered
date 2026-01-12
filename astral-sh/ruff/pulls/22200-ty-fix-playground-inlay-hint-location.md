```yaml
number: 22200
title: "[ty] Fix playground inlay hint location"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: ty-playground-inlay-hint-location
created_at: 2025-12-26T01:47:02Z
updated_at: 2025-12-27T00:21:53Z
url: https://github.com/astral-sh/ruff/pull/22200
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Fix playground inlay hint location

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Previously, we thought this didn't work.

It does, we were just using the wrong variable from `LocationLink`. Range for some reason points roughly to the current location, or the start of the inlay hint label part.

Now we use `targetSelectionRange`, which is what we always needed.

Mentioned in https://github.com/astral-sh/ty/issues/2204

## Test Plan
[Screencast_20251226_014911.webm](https://github.com/user-attachments/assets/1c0b8abb-aa4c-4038-83ee-5e5a90501613)

This does not work at all at [play.ty.dev](https://play.ty.dev/)

---

_Label `playground` added by @MichaReiser on 2025-12-26 08:19_

---

_Label `ty` added by @MichaReiser on 2025-12-26 08:19_

---

_@MichaReiser approved on 2025-12-26 08:20_

Nice find! Thank you

---

_Merged by @MichaReiser on 2025-12-26 08:20_

---

_Closed by @MichaReiser on 2025-12-26 08:20_

---

_Branch deleted on 2025-12-27 00:21_

---

```yaml
number: 13262
title: "Playground: Fix errors not shown on page load"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - playground
assignees: []
merged: true
base: main
head: micha/playground-show-errors-on-load
created_at: 2024-09-06T08:00:10Z
updated_at: 2024-09-09T10:47:41Z
url: https://github.com/astral-sh/ruff/pull/13262
synced_at: 2026-01-12T15:55:43Z
```

# Playground: Fix errors not shown on page load

---

_@MichaReiser_

## Summary

This PR fixes a bug in the playground where errors for a restored snipped didn't show on page load. 

The core issue was that `editor` or `editor.getModels` was `None` when the diagnostics are updated, in which case the `useEffect` skips the diagnostics change. 

This PR fixes the issue by:

* Using `onMount` instead of `beforeMount` to only set the editor instance when the editor is fully loaded
* Setting the diagnostics as part of the `handleMount` callback because the mount event could be triggered after are updated

I used this as a chance to only register a single code action provider. 

Fixes https://github.com/astral-sh/ruff/issues/11918

## Test Plan

[Screencast from 2024-09-06 10-00-24.webm](https://github.com/user-attachments/assets/7d5b5bb5-ab93-4011-a7ab-34d7868d8233)


---

_Label `bug` added by @MichaReiser on 2024-09-06 08:00_

---

_Label `playground` added by @MichaReiser on 2024-09-06 08:00_

---

_Merged by @MichaReiser on 2024-09-09 10:47_

---

_Closed by @MichaReiser on 2024-09-09 10:47_

---

_Branch deleted on 2024-09-09 10:47_

---

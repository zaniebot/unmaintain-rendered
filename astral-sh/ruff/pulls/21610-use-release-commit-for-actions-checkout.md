```yaml
number: 21610
title: Use release commit for actions/checkout
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/dist-use-release-revision
created_at: 2025-11-24T07:58:09Z
updated_at: 2025-11-24T08:24:25Z
url: https://github.com/astral-sh/ruff/pull/21610
synced_at: 2026-01-10T16:48:02Z
```

# Use release commit for actions/checkout

---

_Pull request opened by @MichaReiser on 2025-11-24 07:58_

## Summary

Unfortunately, Renovate doesn't handle pinned actions correctly when the version comment is missing. Instead of using the pinned commit of the latest release, it uses the latest commit to the action's repository, which isn't what we want.


This PR changes the pin of `actions/checkout` from c2d88d3ecc89a9ef08eebf45d9637801dcee7eb5  (one commit after the release) to `1af3b93b6815bc44a9784bd300feb67ff0d1eeb3` (the commit from the release page)

## Test Plan

CI


---

_Label `release` added by @MichaReiser on 2025-11-24 07:58_

---

_Merged by @MichaReiser on 2025-11-24 08:24_

---

_Closed by @MichaReiser on 2025-11-24 08:24_

---

_Branch deleted on 2025-11-24 08:24_

---

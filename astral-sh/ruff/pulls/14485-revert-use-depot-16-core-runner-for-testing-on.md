```yaml
number: 14485
title: "Revert \"Use Depot 16-core runner for testing on Linux\""
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ci
assignees: []
base: main
head: revert-14460-zb/ci-depot-linux
created_at: 2024-11-20T11:11:14Z
updated_at: 2024-11-20T14:22:22Z
url: https://github.com/astral-sh/ruff/pull/14485
synced_at: 2026-01-10T20:50:57Z
```

# Revert "Use Depot 16-core runner for testing on Linux"

---

_Pull request opened by @AlexWaygood on 2024-11-20 11:11_

Reverts astral-sh/ruff#14460

The Linux CI job has been queued for 20 minutes on https://github.com/astral-sh/ruff/pull/14383, and this is the obvious thing that's changed recently with respect to our Linux CI

---

_Label `ci` added by @AlexWaygood on 2024-11-20 11:11_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-20 11:11_

---

_Review requested from @zanieb by @AlexWaygood on 2024-11-20 11:11_

---

_Marked ready for review by @AlexWaygood on 2024-11-20 11:11_

---

_Comment by @AlexWaygood on 2024-11-20 11:14_

This fixes things; the linux job started almost immediately on this PR

---

_Comment by @github-actions[bot] on 2024-11-20 11:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-11-20 11:53_

Let's give @zanieb some time to chime in before we roll back unless this blocks or slows you down right now.

---

_Comment by @AlexWaygood on 2024-11-20 11:56_

> Let's give @zanieb some time to chime in before we roll back unless this blocks or slows you down right now.

It means we can't merge the `ruff-0.8` branch, since the Linux job is a required check, and the Linux job has now been queued for over an hour on https://github.com/astral-sh/ruff/pull/14383. It doesn't block me, but it might block you, since you mentioned that you wanted to merge the branch so that you could get started writing the changelog.

---

_Comment by @MichaReiser on 2024-11-20 11:59_

oh lol... I assumed it's just slower. Not that it's queuing forever 

---

_Comment by @AlexWaygood on 2024-11-20 12:17_

hmm, so the jobs eventually started after being queued for over an hour... and now we have fast CI times again.

I'm okay with closing this for now and revisiting if it happens again (maybe it was a one-time fluke??)

---

_Comment by @zanieb on 2024-11-20 14:21_

Sorry! Figuring out how often we run into these problems is sort of a goal of scaling up our usage of Depot. I'd prefer not to revert this, though I'm sorry it didn't work for a bit.

---

_Closed by @AlexWaygood on 2024-11-20 14:22_

---

_Branch deleted on 2024-11-20 14:22_

---

_Comment by @AlexWaygood on 2024-11-20 14:22_

No worries! Let's hope it stays fast now 🤞

---

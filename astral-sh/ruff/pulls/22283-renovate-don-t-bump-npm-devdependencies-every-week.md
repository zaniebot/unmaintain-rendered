```yaml
number: 22283
title: "renovate: don't bump npm devDependencies every week"
type: pull_request
state: closed
author: woodruffw
labels:
  - internal
assignees: []
base: main
head: ww/no-npm-devdeps-bump
created_at: 2025-12-29T16:50:14Z
updated_at: 2025-12-30T13:50:25Z
url: https://github.com/astral-sh/ruff/pull/22283
synced_at: 2026-01-10T16:36:19Z
```

# renovate: don't bump npm devDependencies every week

---

_Pull request opened by @woodruffw on 2025-12-29 16:50_

## Summary

This weekly bump is noisy and overwrites our range constraints, which serves no purpose since we're locking our JS dependencies anyways.

See #22248 for context.

## Test Plan

NFC.

---

_Review requested from @MichaReiser by @woodruffw on 2025-12-29 16:50_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-29 16:50_

---

_Label `internal` added by @woodruffw on 2025-12-29 16:50_

---

_Review comment by @MichaReiser on `.github/renovate.json5`:84 on 2025-12-29 17:04_

Can we keep the grouping of npm dependencies or do we have this at the org level?

I prefer them grouped because I have to manually checkout the PR and verify that the dev dependencies keep working, which is a bit annoying if I have to do that separately for all dev dependencies 

---

_@MichaReiser reviewed on 2025-12-29 17:04_

---

_@woodruffw reviewed on 2025-12-29 17:17_

---

_Review comment by @woodruffw on `.github/renovate.json5`:84 on 2025-12-29 17:17_

We can keep it! I might have been confused by the problem in #22248 -- do you still want these PRs? I thought you didn't, but maybe you were saying that you want them, just without the range changes?

---

_@woodruffw reviewed on 2025-12-29 17:19_

---

_Review comment by @woodruffw on `.github/renovate.json5`:84 on 2025-12-29 17:19_

Actually yeah, I think I can fix this with the org-wide preset. One sec.

---

_Review comment by @MichaReiser on `.github/renovate.json5`:84 on 2025-12-29 17:19_

I want to keep the PRs and the grouping, if possible, I'm only unsure if we need the pinning in the `package.json`. But I don't mind much.

---

_@MichaReiser reviewed on 2025-12-29 17:19_

---

_@woodruffw reviewed on 2025-12-29 17:20_

---

_Review comment by @woodruffw on `.github/renovate.json5`:84 on 2025-12-29 17:20_

Okay, opened https://github.com/astral-sh/renovate-config/pull/7. If that works we can close this 🙂 

---

_Review comment by @woodruffw on `.github/renovate.json5`:84 on 2025-12-29 17:21_

> I want to keep the PRs and the grouping, if possible, I'm only unsure if we need the pinning in the `package.json`. But I don't mind much.

Makes sense! I think the PR above will accomplish that and will let us retain the grouping.

---

_@woodruffw reviewed on 2025-12-29 17:21_

---

_@MichaReiser reviewed on 2025-12-30 08:27_

---

_Review comment by @MichaReiser on `.github/renovate.json5`:84 on 2025-12-30 08:27_

Can we then close this PR?

---

_@woodruffw reviewed on 2025-12-30 13:50_

---

_Review comment by @woodruffw on `.github/renovate.json5`:84 on 2025-12-30 13:50_

Yep!

---

_Closed by @woodruffw on 2025-12-30 13:50_

---

_Branch deleted on 2025-12-30 13:50_

---

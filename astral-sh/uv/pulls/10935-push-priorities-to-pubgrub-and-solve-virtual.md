```yaml
number: 10935
title: Push priorities to pubgrub and solve virtual package with the strongest constraints first
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
draft: true
base: main
head: konsti/sync-priorities
created_at: 2025-01-24T13:25:30Z
updated_at: 2025-03-10T22:52:11Z
url: https://github.com/astral-sh/uv/pull/10935
synced_at: 2026-01-10T11:10:34Z
```

# Push priorities to pubgrub and solve virtual package with the strongest constraints first

---

_Pull request opened by @konstin on 2025-01-24 13:25_

We were updating package priorities in without syncing them with pubgrub, so uv and pubgrub were getting out of sync. Surprisingly, this didn't cause any apparent differences in resolution, I expected this would change at least our ecosystem cases. It's potentially breaking through any time we change something in either uv or pubgrub.

We fix this by pushing updates to pubgrub. We couldn't previously u this because the way priority updates were tracked through the decision level. Instead, we switch to tracking the packages whose derivations changed in a set, based on https://github.com/pubgrub-rs/pubgrub/compare/dev...Eh2406:pubgrub:stop-prioritize. Since uv priorities don't depend on the ranges, we can speed this up further by not using this set in uv. In pubgrub, we can upstream this and use it as a correctness fix to also reprioritize when the conflict tracker changed, which is currently not handled.

I thought I had a performance regression that would be fixed by making pubgrub priorities fast by using the package ids as index. This turned out to be insufficient and the perf bottleneck had to be fixed on the pubgrub side.

Inside a package, we switch to processing the virtual package with the strongest constraint first, to avoid missing constraints:

Say we first see `a; python_version ">=3.9"` and then `a==1; python_version ">=3.10"`. We would currently assign a the wrong version (e.g. `a==2` ) through deciding `a; python_version ">=3.9"` first, then fix it up when we see `a==1; python_version ">=3.10"` right after.

This is a potentially breaking change in that it can change the resolution in edge cases.

---

_Label `bug` added by @konstin on 2025-01-24 13:25_

---

_Label `performance` added by @konstin on 2025-01-24 13:25_

---

_Label `performance` removed by @konstin on 2025-01-24 13:39_

---

_Comment by @konstin on 2025-03-10 22:52_

Surprisingly, this PR doesn't change resolutions we observe, it only changes the order of virtual packages: The order of the package names is already correct as far as i can tell (events that lead uv to updating the priority for a package name also lead pubgrub to re-query that packages priority). It still makes sense to push updates to the virtual package priorities in pubgrub when they change in uv to avoid subtle problems in the future, though not in the shape of this PR.

---

_Closed by @konstin on 2025-03-10 22:52_

---

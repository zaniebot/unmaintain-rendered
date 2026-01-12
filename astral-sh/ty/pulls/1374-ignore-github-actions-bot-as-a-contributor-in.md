```yaml
number: 1374
title: "Ignore `github-actions` bot as a contributor in CHANGELOG"
type: pull_request
state: merged
author: sharkdp
labels:
  - release
assignees: []
merged: true
base: main
head: david/ignore-github-actions-bot
created_at: 2025-10-16T18:23:53Z
updated_at: 2025-10-16T19:02:00Z
url: https://github.com/astral-sh/ty/pull/1374
synced_at: 2026-01-12T15:54:27Z
```

# Ignore `github-actions` bot as a contributor in CHANGELOG

---

_@sharkdp_

Re: https://github.com/astral-sh/ty/pull/1371#discussion_r2436914106

I *think* this should work?

https://github.com/zanieb/rooster/blob/b6dafd2c8dc596680bb9e91a1c52192a5ecc3a9e/src/rooster/_config.py#L43

https://github.com/zanieb/rooster/blob/b6dafd2c8dc596680bb9e91a1c52192a5ecc3a9e/src/rooster/_changelog.py#L278-L282

I don't think we need to exclude dependabot (or renovate) because those PRs are labeled as `internal` anyway.

---

_Label `release` added by @sharkdp on 2025-10-16 18:23_

---

_Review requested from @zanieb by @AlexWaygood on 2025-10-16 18:25_

---

_Comment by @AlexWaygood on 2025-10-16 18:26_

to double-check my understanding: the effect we're aiming for here is that the PRs will be listed in the changelog (because typeshed syncs are user-facing changes) but the author of the PRs (github-actions) will not be listed as one of the contributors?

---

_Comment by @sharkdp on 2025-10-16 18:27_

> to double-check my understanding: the effect we're aiming for here is that the PRs will be listed in the changelog (because typeshed syncs are user-facing changes) but the author of the PRs (github-actions) will not be listed as one of the contributors?

Correct

---

_Renamed from "Ignore github-actions bot for CHANGELOGs" to "Ignore `github-actions` bot as a contributor in CHANGELOG" by @sharkdp on 2025-10-16 18:28_

---

_@zanieb approved on 2025-10-16 18:51_

---

_Merged by @sharkdp on 2025-10-16 19:01_

---

_Closed by @sharkdp on 2025-10-16 19:01_

---

_Branch deleted on 2025-10-16 19:02_

---

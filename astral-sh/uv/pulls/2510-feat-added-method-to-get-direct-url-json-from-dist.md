```yaml
number: 2510
title: "feat: added method to get direct url json from dist"
type: pull_request
state: closed
author: tdejager
labels: []
assignees: []
base: main
head: feat/add-direct-url-json
created_at: 2024-03-18T13:39:06Z
updated_at: 2024-03-21T13:37:34Z
url: https://github.com/astral-sh/uv/pull/2510
synced_at: 2026-01-12T16:05:05Z
```

# feat: added method to get direct url json from dist

---

_@tdejager_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
We want to use this in `pixi` to check the installed info against the lock file.

## Test Plan

<!-- How was it tested? -->
Not added any tests yet. I know uv writes the `direct_url.json`. I can add a test if advised :)

---

_@charliermarsh reviewed on 2024-03-18 17:23_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:199 on 2024-03-18 17:23_

Can you not use this method directly in Pixi?

---

_@tdejager reviewed on 2024-03-18 18:37_

---

_Review comment by @tdejager on `crates/distribution-types/src/installed.rs`:199 on 2024-03-18 18:37_

For sure thats what I did :). However, other files can be accessed like `installer`, `metadata` etc. This one is the only one thats not public, so I thought it might make sense to expose.

EDIT:
I meant to say I vendored the `InstalledDist::direct_url` into pixi.

---

_@tdejager reviewed on 2024-03-18 18:41_

---

_Review comment by @tdejager on `crates/distribution-types/src/installed.rs`:199 on 2024-03-18 18:41_

Also, if I'm readin the code correctly, I don't believe you are checking the `direct_url.json` yet, but I think you might want to for the `pip_sync` functionality instead of only relying on the freshness of the cache <-> disk?

---

_Review comment by @tdejager on `crates/distribution-types/src/installed.rs`:199 on 2024-03-21 08:24_

Hey @charliermarsh, any thoughts on this? The `InstalledDist::direct_url` is not public either, not sure if that was clear. I've made A PR for that if you like that more: https://github.com/astral-sh/uv/pull/2584 feel free to close this one.

---

_@tdejager reviewed on 2024-03-21 08:24_

---

_@charliermarsh reviewed on 2024-03-21 13:37_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:199 on 2024-03-21 13:37_

Okay thanks, I prefer that PR, sorry for the delay!

---

_Closed by @charliermarsh on 2024-03-21 13:37_

---

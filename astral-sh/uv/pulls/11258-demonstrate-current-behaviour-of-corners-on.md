```yaml
number: 11258
title: demonstrate current behaviour of corners on current main
type: pull_request
state: closed
author: Gankra
labels:
  - internal
assignees: []
draft: true
base: main
head: gankra/test-corners
created_at: 2025-02-05T19:47:18Z
updated_at: 2025-02-05T20:10:19Z
url: https://github.com/astral-sh/uv/pull/11258
synced_at: 2026-01-12T16:09:45Z
```

# demonstrate current behaviour of corners on current main

---

_@Gankra_

This is the same test commit in #11224 but with an extra commit added where i regen them on main to show how they differ.

You can see that they largely error in the same places except:

* my PR turns some panics into proper errors
* my PR newly forbids `--all-groups --only-dev` while on main it's nonsensically kinda `--only-all-groups` but just because it's not handled properly and different APIs give different answers
* my PR newly interprets `--all-groups --no-default-groups` as `--all-groups`, while on main it's nonsensically being interpretted as `--no-default-groups`.
* they both agree `--dev --only-dev` saturates to `--only-dev` (the snapshot churns because of the previous difference, the net result is the same)

---

_Label `internal` added by @Gankra on 2025-02-05 19:47_

---

_Closed by @Gankra on 2025-02-05 20:10_

---

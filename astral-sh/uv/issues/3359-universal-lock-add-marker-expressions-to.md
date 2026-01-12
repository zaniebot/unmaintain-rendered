```yaml
number: 3359
title: "universal-lock: add marker expressions to PubGrubPackage"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-03T18:15:29Z
updated_at: 2024-05-20T23:56:25Z
url: https://github.com/astral-sh/uv/issues/3359
synced_at: 2026-01-12T15:58:43Z
```

# universal-lock: add marker expressions to PubGrubPackage

---

_@BurntSushi_

As part of #3350, we'll need to distinguish conflicting dependencies among forks. We can achieve this by adding the relevant marker expressions to `PubGrubPackage`:

https://github.com/astral-sh/uv/blob/e33ff95575e8165078799548070210947d1cfad0/crates/uv-resolver/src/pubgrub/package.rs#L22-L63

I did this in my prototype by adding them to `PubGrubPackage::Package`:

https://github.com/astral-sh/uv/blob/7f2b401260e6231bebb867c034b27d2955210fe0/crates/uv-resolver/src/pubgrub/package.rs#L50-L53

But I think it might be worth experimenting with creating a new variant. Note that like extras, a package with a non-None marker expression should introduce a dependency on the package with the same name, but without the marker expressions:

https://github.com/astral-sh/uv/blob/7f2b401260e6231bebb867c034b27d2955210fe0/crates/uv-resolver/src/resolver/mod.rs#L1031-L1043

This adds the necessary constraints to ensure pubgrub's resolution is itself properly constrained.

---

_Label `preview` added by @BurntSushi on 2024-05-03 18:15_

---

_Closed by @BurntSushi on 2024-05-20 23:56_

---

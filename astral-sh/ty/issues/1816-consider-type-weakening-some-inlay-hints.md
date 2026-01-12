```yaml
number: 1816
title: consider type weakening some inlay-hints
type: issue
state: open
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-12-09T00:38:26Z
updated_at: 2026-01-11T09:47:33Z
url: https://github.com/astral-sh/ty/issues/1816
synced_at: 2026-01-12T15:54:25Z
```

# consider type weakening some inlay-hints

---

_@Gankra_

`Literal` inlay hints are often extremely verbose and not what you would ever want to bake. We could consider applying a "weakening" transform to some of them:

* Literal["foo"] -> str
* Literal[1] -> int

etc.

(Writing this down before I forget about it)

---

_Label `server` added by @Gankra on 2025-12-09 00:38_

---

_Comment by @AlexWaygood on 2025-12-09 00:40_

Elsewhere in ty we refer to this as "Literal promotion" :-)

---

_Comment by @carljm on 2025-12-09 00:46_

Here also it seems potentially quite confusing if inlay hints do not match the type we actually infer and will use for subsequent uses of the name. I've seen a lot of evidence already in issues that users are using inlay hints as a replacement for `reveal_type`. I'm not sure that we should sacrifice this fidelity in the interests of making inlay hints more bakeable.

---

_Comment by @Gankra on 2025-12-09 00:49_

Possibly a setting we could expose?

---

_Comment by @Lexicality on 2026-01-11 08:59_

<img width="677" height="149" alt="Image" src="https://github.com/user-attachments/assets/b2e448fb-fc48-479c-ab46-8b1734ec4a64" />

I ran into this recently and ended up having to drop a `: str` on the variable just to avoid cluttering up my screen.

I agree that changing the inferred type would be potentially confusing, but maybe limiting the size of strings? eg `Literal["Lorem ipsum..."]`

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-11 09:47_

---

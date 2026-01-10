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
updated_at: 2025-12-09T00:49:40Z
url: https://github.com/astral-sh/ty/issues/1816
synced_at: 2026-01-10T01:56:41Z
```

# consider type weakening some inlay-hints

---

_Issue opened by @Gankra on 2025-12-09 00:38_

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

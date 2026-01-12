```yaml
number: 1604
title: support recursive PEP 613 / implicit type aliases
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-11-21T00:03:59Z
updated_at: 2025-12-11T09:12:51Z
url: https://github.com/astral-sh/ty/issues/1604
synced_at: 2026-01-12T15:54:25Z
```

# support recursive PEP 613 / implicit type aliases

---

_@carljm_

Currently these typically fall back to Divergent/Unknown at the point of recursion. We should properly support them.

An example case where this is relied on in typeshed is the annotation for the second argument to `isinstance`, which is a recursive tuple-of-tuples. Currently we don't catch the type error in `isinstance(None, (int, None))` (though we do catch it in `isinstance(None, None)`), due to not fully supporting the recursion.

---

_Added to milestone `Stable` by @carljm on 2025-11-21 00:03_

---

_Label `typing semantics` added by @dhruvmanila on 2025-11-21 04:37_

---

_Comment by @sharkdp on 2025-12-11 09:12_

Closing in favor of https://github.com/astral-sh/ty/issues/1738 which I accidentally opened afterwards, but with slightly more information. I will carry over the typeshed example.

---

_Closed by @sharkdp on 2025-12-11 09:12_

---

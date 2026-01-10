```yaml
number: 13854
title: "[red-knot] Compare expression inference - Intersection"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-10-21T10:43:15Z
updated_at: 2024-11-07T19:51:15Z
url: https://github.com/astral-sh/ruff/issues/13854
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] Compare expression inference - Intersection

---

_Issue opened by @sharkdp on 2024-10-21 10:43_

As part of #13618, implement inference for compare expressions involving intersection types.

---

_Renamed from "Intersection" to "[red-knot] Compare expression inference - Intersection" by @sharkdp on 2024-10-21 10:43_

---

_Assigned to @sharkdp by @sharkdp on 2024-10-21 10:43_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-21 10:48_

---

_Comment by @carljm on 2024-11-07 15:20_

@sharkdp What is the status here? It would be nice to just close out the compare-expression task, but I also think this is probably a low-priority edge case. Is there an imprecise-but-not-incorrect approach we could put in place for now, pending real use cases, and call this done?

---

_Comment by @sharkdp on 2024-11-07 15:30_

@carljm The status is that I have [this draft](https://github.com/astral-sh/ruff/pull/14138). It's better than nothing in that it turns a lot of `Type::Todo`s into correct answers, but it feels unfinished and potentially even wrong in some cases.

Glad to talk about it.

---

_Comment by @carljm on 2024-11-07 17:26_

We discussed offline - the PR looks pretty good to me (it's OK if we are correct but not as precise as we could theoretically be) so we'll get it wrapped up and landed soon.

---

_Closed by @sharkdp on 2024-11-07 19:51_

---

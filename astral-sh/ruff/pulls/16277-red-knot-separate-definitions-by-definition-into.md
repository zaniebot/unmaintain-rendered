```yaml
number: 16277
title: "[red-knot] Separate `definitions_by_definition` into separate fields"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/defs-by-defs
created_at: 2025-02-20T14:32:56Z
updated_at: 2025-02-20T18:28:27Z
url: https://github.com/astral-sh/ruff/pull/16277
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Separate `definitions_by_definition` into separate fields

---

_Pull request opened by @dcreager on 2025-02-20 14:32_

A minor cleanup that breaks up a `HashMap` of an enum into separate `HashMap`s for each variant.  (These separate fields were already how this cache was being described in the big comment at the top of the file!)

---

_Label `internal` added by @dcreager on 2025-02-20 14:32_

---

_Label `red-knot` added by @dcreager on 2025-02-20 14:32_

---

_Review requested from @carljm by @dcreager on 2025-02-20 14:32_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-20 14:32_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-20 14:32_

---

_Review requested from @sharkdp by @dcreager on 2025-02-20 14:32_

---

_Comment by @MichaReiser on 2025-02-20 14:39_

Is there any other motivation other than that this is how it was described in the comment? 

I assume the idea of using a single hash map is to reduce the need for allocations (and make the struct slightly smaller). I don't know if this is a worthwhile trade-off.

---

_@MichaReiser approved on 2025-02-20 14:39_

It does seem to make codspeed hapy

---

_Comment by @dcreager on 2025-02-20 14:46_

> Is there any other motivation other than that this is how it was described in the comment?
> 
> I assume the idea of using a single hash map is to reduce the need for allocations (and make the struct slightly smaller). I don't know if this is a worthwhile trade-off.

It was partly that and partly that the code seemed cleaner — the enum only existed to be able to fold both cases into a single hashmap, but they really are conceptually different caches that seemed better to store separately.  (I would have considered this worthwhile if performance was a wash, but not if it regressed)

---

_Merged by @dcreager on 2025-02-20 14:47_

---

_Closed by @dcreager on 2025-02-20 14:47_

---

_Branch deleted on 2025-02-20 14:47_

---

_Comment by @carljm on 2025-02-20 18:28_

Thank you! Yeah I was just trying to reduce the number of hashmaps, but if codspeed is happy with this, it's definitely nicer.

---

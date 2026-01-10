---
number: 2428
title: wrong inference of bound-method / function literals off nominal-instance / subclass-of types
type: issue
state: open
author: carljm
labels:
  - callables
assignees: []
created_at: 2026-01-09T22:12:10Z
updated_at: 2026-01-09T22:13:19Z
url: https://github.com/astral-sh/ty/issues/2428
synced_at: 2026-01-10T01:48:23Z
---

# wrong inference of bound-method / function literals off nominal-instance / subclass-of types

---

_Issue opened by @carljm on 2026-01-09 22:12_

> I think our inference of singleton function or bound-method types when accessed off nominal-instance or subclass-of types is simply wrong and needs to be fixed. The nominal-instance type `A` includes instances of `A` and all subclasses of `A`. That means it is wrong to say that `instance_of_a.method` is a singleton bound-method type, unless either `A` or `A.method` is marked as final. Similarly, `type[A]` includes subclasses of `A`, so `subclass_of_A.method` cannot be a singleton function literal type.
> 
> Accessing methods as attributes of class-literal types can still correctly return a function literal.
> 
> One unfortunate side effect of making the above fix is that we will lose go-to-definition on most methods. To avoid this regression, we may need to allow callable types to optionally carry a "most precise known" source definition (or definitions?), which is not "part of the type" in terms of type relations but can be used for go-to-definition. 

 _Originally posted by @carljm in [#770](https://github.com/astral-sh/ty/issues/770#issuecomment-3382323817)_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 22:12_

---

_Label `callables` added by @carljm on 2026-01-09 22:12_

---

_Removed from milestone `Stable` by @carljm on 2026-01-09 22:12_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-09 22:12_

---

_Comment by @carljm on 2026-01-09 22:13_

I'm putting this in Pre-stable milestone mostly because of the impact on the LSP, which means it's better we address this sooner than later.

---

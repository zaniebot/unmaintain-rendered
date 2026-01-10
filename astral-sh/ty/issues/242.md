```yaml
number: 242
title: ensure that type inference on a high-durability file is also high durability
type: issue
state: open
author: carljm
labels:
  - performance
assignees: []
created_at: 2024-08-30T19:50:15Z
updated_at: 2025-05-07T15:27:38Z
url: https://github.com/astral-sh/ty/issues/242
synced_at: 2026-01-10T02:34:09Z
```

# ensure that type inference on a high-durability file is also high durability

---

_Issue opened by @carljm on 2024-08-30 19:50_

If we can ensure this is the case, it should help our incremental inference perform better.

One issue here is that low-durability files (in your workspace) can shadow high-durability files (in the stdlib), and in principle this could impact the type inference in the stdlib. This means that any `resolve_module` is (I think) currently a low-durability query, because it will always access some low-durability import search paths.

If it helps performance enough, we could decide that we never want type inference within the stdlib to respect shadowed stdlib modules, so that we can keep type inference of the stdlib all high-durability, and not need to revalidate it in depth on incremental changes to user code.

This maybe should go hand-in-hand with always warning or erroring if a user module _does_ shadow a stdlib module.

---

_Label `performance` added by @carljm on 2024-08-30 19:51_

---

_Comment by @MichaReiser on 2024-09-01 11:19_

So, the issue here is that the high durability of stdlib files doesn't yield its full potential because the type inference result of any module with imports depends on `resolve_module` which always has durability low (because we first start resolving local paths)?

One alternative is to give `File::status` a higher durability. That's the only field `resolve_module` depends on. 
Another alternative is to move the module resolver out of salsa and model `Module`s as inputs. But that has obvious downsides. 

---

_Comment by @carljm on 2024-09-01 15:28_

> So, the issue here is that the high durability of stdlib files doesn't yield its full potential because the type inference result of any module with imports depends on `resolve_module` which always has durability low (because we first start resolving local paths)?

Yes, that's right.

> One alternative is to give `File::status` a higher durability. That's the only field `resolve_module` depends on.

Wouldn't that mean that adding or removing a file in your workspace root would require re-checking all high-durability queries? I guess maybe that could be fine, assuming adding or removing files doesn't happen that often.

The solution I was proposing was that we would instead have imports from the stdlib not even consider search paths other than the stdlib. Which is technically wrong (since shadowing is possible), but practically right if we consider shadowing the stdlib itself to be an error (which I think pyright at least does.)

---

_Comment by @MichaReiser on 2024-09-02 07:40_

> Wouldn't that mean that adding or removing a file in your workspace root would require re-checking all high-durability queries? I guess maybe that could be fine, assuming adding or removing files doesn't happen that often.

Yes, it doesn't solve the problem entirely, but it might be a desired change anyway because it avoids a deep-verify for all third-party code whenever a first-party module is changed. 

> The solution I was proposing was that we would instead have imports from the stdlib not even consider search paths other than the stdlib. Which is technically wrong (since shadowing is possible), but practically right if we consider shadowing the stdlib itself to be an error (which I think pyright at least does.)

That makes sense. I think we may want to do both where what I suggested reduces deep-verification for third-party code and your suggestion avoids deep-verification for stdlib modules. The only thing we need to be mindful of is that forbidding stdlib modules altogether may be overly-strict for some projects. 

---

_Renamed from "[red-knot] ensure that type inference on a high-durability file is also high durability" to "ensure that type inference on a high-durability file is also high durability" by @MichaReiser on 2025-05-07 15:27_

---

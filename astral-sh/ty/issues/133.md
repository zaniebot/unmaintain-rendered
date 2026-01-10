```yaml
number: 133
title: Model implicit imports of submodules, if it occurs in a parent module __init__.py
type: issue
state: closed
author: sharkdp
labels:
  - imports
  - runtime semantics
assignees: []
created_at: 2025-04-08T13:11:52Z
updated_at: 2025-11-12T15:33:45Z
url: https://github.com/astral-sh/ty/issues/133
synced_at: 2026-01-10T02:06:24Z
```

# Model implicit imports of submodules, if it occurs in a parent module __init__.py

---

_Issue opened by @sharkdp on 2025-04-08 13:11_

Consider the following case **([playground](https://playknot.ruff.rs/68b7810b-06b5-4914-b0a2-8bc1d5513181)):**

`main.py`

```py
import module

# Missing explicit import of submodule:
# import module.submodule

print(module.submodule.SubmoduleClass)
```

`module/__init__.py`

```py
from module.submodule import SubmoduleClass
```

`module/submodule.py`

```py
class SubmoduleClass: ...
```

This works at runtime due to a quirk in module resolution (the `from module.submodule import SubmoduleClass` import implicitly stores `submodule` as an attribute on `module`, thanks @AlexWaygood).

It is not considered good practice to rely on this, but people will probably write code that does so. Neither pyright nor mypy report any errors here. Should we attempt to model this behavior as well?

---

_Comment by @carljm on 2025-04-09 13:44_

Do mypy/pyright emit errors on the same scenario, but without the submodule import in `__init__.py`?

That is, are they actually modeling the import side effect, or are they simply failing to catch this category of error altogether?

~~My feeling is that the code as written only works by non-local "accident", and there is only upside to requiring an explicit import of the submodule in the module where it is used. So I like our current behavior.~~

EDIT: On further reflection, I think there could be a case for handling this, specifically in the case shown where the module's own `__init__.py` always imports the submodule. This is more robust than the general case, and more plausible for us to detect.

The more general case is that this scenario can occur, and work at runtime, with the submodule import located literally anywhere in the codebase, as long as it's in a module that happens to get imported before the submodule access in `main.py` occurs. But this is extremely fragile, as order of imports can easily change unintentionally. I don't think we should (or can) handle that general case. The only way we could, I think, would be to always assume all submodules are available as attributes. That is, give up on catching this class of error entirely.

---

_Comment by @sharkdp on 2025-04-09 17:31_

*With* the `from module.submodule import SubmoduleClass` import in `module/__init__.py`:

* Mypy: no error
* Pyright: main.py:6:14 - error: "submodule" is not a known attribute of module "module"
* Red Knot: main.py:6:7: Type `<module 'module'>` has no attribute `submodule`

*Without* the `from module.submodule import SubmoduleClass` import in `module/__init__.py`:

* Exactly the same behavior for all type checkers

---

I might have only tried mpyp in the original example. It seems like we are consistent with pyright here. And it seems like `mypy` simply doesn't aim to catch this category of error. So it seems like we can maybe just close this without changing anything?




> EDIT: On further reflection, I think there could be a case for handling this, specifically in the case shown where the module's own `__init__.py` always imports the submodule. This is more robust than the general case, and more plausible for us to detect.

👍 



> The more general case is that this scenario can occur, and work at runtime, with the submodule import located literally anywhere in the codebase, as long as it's in a module that happens to get imported before the submodule access in `main.py` occurs. But this is extremely fragile, as order of imports can easily change unintentionally.

Do not remind me of my C++ times!

> I don't think we should (or can) handle that general case.

Absolutely. In this case, the diagnostic would technically be a "false positive" (it "works" at runtime), but is arguably serving a purpose. Because it allows you to find missing explicit imports that might end up causing trouble.



---

_Comment by @AlexWaygood on 2025-04-09 17:35_

> Absolutely. In this case, the diagnostic would technically be a "false positive" (it "works" at runtime), but is arguably serving a purpose. Because it allows you to find missing explicit imports that might end up causing trouble.

I suppose we could add a subdiagnostic (or at least detailed documentation) explaining this point. I agree that this is a case where I'd personally prefer the false positive over the false negative, but it could be confusing to users that the attribute access "works fine at runtime" but we emit a diagnostic about it anyway

---

_Renamed from "[red-knot] Model implicit imports of submodules?" to "[red-knot] Model implicit imports of submodules, if it occurs in a parent module __init__.py" by @carljm on 2025-04-09 17:53_

---

_Renamed from "[red-knot] Model implicit imports of submodules, if it occurs in a parent module __init__.py" to "[red-knot] Model implicit imports of submodules, if it occurs in a parent module __init__.py?" by @carljm on 2025-04-09 17:53_

---

_Comment by @carljm on 2025-04-09 17:53_

I edited the issue title to reflect (I think) our agreement on what we would change here, if we change anything at all. But I'm comfortable changing nothing at all unless real-world false-positive data convinces us to make this a priority over other things.

---

_Renamed from "[red-knot] Model implicit imports of submodules, if it occurs in a parent module __init__.py?" to "Model implicit imports of submodules, if it occurs in a parent module __init__.py?" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @AlexWaygood on 2025-05-10 17:27_

I just closed https://github.com/astral-sh/ty/issues/313 as a duplicate of this issue. I think we should make the suggested change here.

---

_Label `imports` added by @AlexWaygood on 2025-05-10 18:06_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:37_

---

_Renamed from "Model implicit imports of submodules, if it occurs in a parent module __init__.py?" to "Model implicit imports of submodules, if it occurs in a parent module __init__.py" by @carljm on 2025-05-19 19:17_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:45_

---

_Comment by @carljm on 2025-07-01 00:30_

Example case that should work after `uv add msgspec`:

```py
import msgspec

msgspec.json.decode
```

Should not emit `unresolved-attribute`.

---

_Comment by @erictraut on 2025-07-01 00:39_

> Do mypy/pyright emit errors on the same scenario, but without the submodule import in `__init__.py`?

Mypy seems to be a bit inconsistent here — or I wasn't able to discern the logic behind its choices. With pyright, I tried to be deliberate and principled about which cases I modeled and which I did not. These choices and the reasoning behind them are documented [here](https://microsoft.github.io/pyright/#/import-statements) if you're interested.

---

_Comment by @carljm on 2025-07-01 00:42_

I think this issue corresponds to adding support for item number 2 under "implicit module loads" in the pyright doc?

Except the doc (and @sharkdp 's test above) suggests that pyright might be particular about the import in `__init__.py` using the specific syntactic form `from .a import b` (rather than e.g. `from module.a import b`)? I'm not sure I see a strong case for distinguishing those two, if they resolve to the same submodule.

Other than that, pyright's choices look good to me.

---

_Comment by @AlexWaygood on 2025-07-14 17:22_

> > Do mypy/pyright emit errors on the same scenario, but without the submodule import in `__init__.py`?
> 
> Mypy seems to be a bit inconsistent here — or I wasn't able to discern the logic behind its choices. With pyright, I tried to be deliberate and principled about which cases I modeled and which I did not. These choices and the reasoning behind them are documented [here](https://microsoft.github.io/pyright/#/import-statements) if you're interested.

I agree mypy's logic around this is hard to reason about from a user's perspective. There is a consistent logic of some form being applied, however -- @brianschubert gave a great explanation of how it approaches this issue in https://github.com/python/typeshed/issues/14246#issuecomment-2959398061

I also prefer pyright's choices overall on this matter

---

_Assigned to @Gankra by @carljm on 2025-08-15 14:56_

---

_Closed by @Gankra on 2025-10-31 14:29_

---

_Comment by @AlexWaygood on 2025-10-31 14:45_

Reopening this since the originally reported issue described in https://github.com/astral-sh/ty/issues/133#issue-3046355761 isn't yet fixed (and we should still consider adding support for it)

---

_Reopened by @AlexWaygood on 2025-10-31 14:45_

---

_Comment by @Gankra on 2025-10-31 15:26_

Note: Fixing the originally reported issue should probably *not* be done with available_submodule_attributes, and instead just, more correct handling of the effects of `from..import` in .py files.

---

_Comment by @carljm on 2025-10-31 17:49_

> Reopening this since the originally reported issue described in [#133 (comment)](https://github.com/astral-sh/ty/issues/133#issue-3046355761) isn't yet fixed (and we should still consider adding support for it)

My understanding here is a bit stronger than "we should still consider" -- I think we have already considered it, and it is clear that we should support it, and we should aim to do it for the beta. (That's actually the problem that I thought we were prioritizing for the beta by prioritizing this issue, but it seems like several similar problems got conflated in this issue, and we ended up focusing on a different one first.)

---

_Comment by @danielhollas on 2025-10-31 17:59_

> My understanding here is a bit stronger than "we should still consider" 

Just a random note "from the field": This issue is currently the biggest source of false positives in our codebases. :-) 

---

_Comment by @Gankra on 2025-10-31 18:22_

I will look into what's required to make this test case, which captures the original issue, pass:

https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md#from-import-of-non-submodule-non-stub-check

---

_Closed by @AlexWaygood on 2025-10-31 18:34_

---

_Reopened by @sharkdp on 2025-10-31 18:35_

---

_Comment by @Gankra on 2025-11-05 17:51_

Ok so mea culpa the *exact* issue in OP is *still* not fixed by https://github.com/astral-sh/ruff/pull/21173

But there's a lot of different behaviours we could or could not implement here, so I opted to break out a ton of more specific/fine-grained issues as followups. Many of the followups should be really easy to implement too:

* #1482 
* #1484 
* #1485
* #1486
* #1487
* #1489
* #1490
* #1526

---

_Closed by @Gankra on 2025-11-10 23:04_

---

_Comment by @Gankra on 2025-11-11 18:34_

Ok I believe the original issue is now *actually* fixed.

---

_Comment by @fitz-vivodyne on 2025-11-12 15:33_

If this is in fact fixed the pinned issue can be updated: https://github.com/astral-sh/ty/issues/445

---

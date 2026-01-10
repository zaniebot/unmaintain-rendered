```yaml
number: 19116
title: "[ty] Local type, public type, imported type, attribute type, nonlocal type"
type: pull_request
state: open
author: sharkdp
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: david/rename-public-types
created_at: 2025-07-03T08:35:01Z
updated_at: 2025-07-08T08:54:31Z
url: https://github.com/astral-sh/ruff/pull/19116
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Local type, public type, imported type, attribute type, nonlocal type

---

_Pull request opened by @sharkdp on 2025-07-03 08:35_

## Summary

This PR attempts to clarify the terminology around local types, public types, imported types, attribute types and nonlocal types. I think we now use the "correct" term in all places, but I'm not 100% satisfied with this for two reasons: (1) these are *a lot* of new terms (2) "nonlocal" is not the opposite of "local", "public" is the opposite of "local".

---

_Review requested from @carljm by @sharkdp on 2025-07-03 08:35_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-03 08:35_

---

_Review requested from @dcreager by @sharkdp on 2025-07-03 08:35_

---

_Label `internal` added by @sharkdp on 2025-07-03 08:35_

---

_Label `ty` added by @sharkdp on 2025-07-03 08:35_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:49 on 2025-07-03 08:36_

I don't think it's particularly important to refer to the correct category of type here. This is trying to make a different point. "Public" is too narrow: this would also work for local uses of `f` after this definition.

---

_@sharkdp reviewed on 2025-07-03 08:36_

---

_Comment by @github-actions[bot] on 2025-07-03 08:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~41MB
+     memo fields = ~45MB

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
-     memo fields = ~156MB
+     memo fields = ~171MB

mkosi (https://github.com/systemd/mkosi)
-     memo fields = ~106MB
+     memo fields = ~97MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~66MB
+     memo fields = ~72MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~142MB
+     memo fields = ~129MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~189MB

meson (https://github.com/mesonbuild/meson)
-     memo fields = ~304MB
+     memo fields = ~334MB

```
</details>


---

_@sharkdp reviewed on 2025-07-03 08:41_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/glossary.md`:1 on 2025-07-03 08:41_

Documents like these tend to stagnate over time, so I'm also fine moving this content somewhere else and less central. But I think there would be room for more things that we could explain here. For example:
* Definition vs bindings vs declaration
* Place vs symbol vs qualified types vs types
* "Symbol" in semantic index building vs "symbol" in type inference

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/glossary.md`:78 on 2025-07-03 11:36_

The fact that we need to subdivide the concept of "public types" into three sub-concepts makes me wonder if the "public types" concept is even useful at all at this point? Could we not have four top-level categories (local types; imported types; attribute types; nonlocal types) instead of two top-level categories, of which one has three subcategories?

---

_@AlexWaygood reviewed on 2025-07-03 11:36_

thanks, this is useful!

---

_@sharkdp reviewed on 2025-07-03 11:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/glossary.md`:78 on 2025-07-03 11:54_

> The fact that we need to subdivide the concept of "public types" into three sub-concepts makes me wonder if the "public types" concept is even useful at all at this point?

"public type" is still used in many places in the codebase and in tests for that exact purpose. We treat all public types together in a special way when it comes to this union-with-Unknown thing. So I'd rather get rid of "imported types" and "attribute types" then to get rid of "public types", I think?

Otherwise I have to use "imported types or attribute types or nonlocal types" everywhere (I can't use "all types that are not local" because that sounds too much like nonlocal).

---

_@AlexWaygood reviewed on 2025-07-03 12:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/glossary.md`:78 on 2025-07-03 12:41_

Hmm, okay, we can leave this as-is then :-)

---

_@AlexWaygood approved on 2025-07-03 13:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/glossary.md`:63 on 2025-07-03 16:53_

I think these statements are not true of all public types. Specifically for [nonlocal use which resolves to an enclosing function scope](https://play.ty.dev/14b48f87-c21e-4a3c-9e8a-ed50ac07ce8d) -- in this case we _can_ see all potential bindings, we just don't know which one is most recent at the time we are called, so we do union all reachable definitions, but we don't union with unknown.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/glossary.md`:70 on 2025-07-03 16:55_

Maybe worth mentioning in passing here that for imported types, that scope will always be a module global scope (by the nature of imports)
```suggestion
    imported type always refers to the local type that the symbol would have at the *end of the
    module scope* in which it was defined.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/glossary.md`:78 on 2025-07-03 16:58_

> We treat all public types together in a special way when it comes to this union-with-Unknown thing.

Even this isn't true of all public types, as I mentioned in another comment above.

(But I'm still OK with keeping the blanket term, if there are places we use it that correctly apply to all kinds of public types.)

---

_@carljm approved on 2025-07-03 17:01_

Thank you!

I agree that I don't love the term "nonlocal", both because it seems like it should describe everything that isn't a "local type", and because it seems like it might be connected to the `nonlocal` keyword, which it is in a sort of general semantic sense, but it's applicability isn't limited to cases where the `nonlocal` keyword is used.

But I also don't have a better idea for what term to use there: "nonlocal" seems to clearly describe what we are talking about.

---

_Converted to draft by @sharkdp on 2025-07-08 08:54_

---

```yaml
number: 1756
title: Completions should be more discriminating with symbols in stub files
type: issue
state: open
author: BurntSushi
labels:
  - server
  - stubs
  - completions
assignees: []
created_at: 2025-12-04T15:33:50Z
updated_at: 2025-12-04T15:39:21Z
url: https://github.com/astral-sh/ty/issues/1756
synced_at: 2026-01-12T15:54:25Z
```

# Completions should be more discriminating with symbols in stub files

---

_@BurntSushi_

This came up [here](https://github.com/astral-sh/ruff/pull/21779/files/d314aa859dcf9a6ffb006a594d7ca0431dd4fab2#r2586899735).

@carljm said:

> I think this is the right call for Python files, but I'm not sure about stubs / typeshed. I think in the case of stubs we maybe shouldn't auto-complete non-re-exported names at all? But not totally sure, and it's not a question that has to be answered in this PR.

And @AlexWaygood said:

> You should assume that most names in a stub also exist at runtime.
> 
> Private names in a stub (starting with a single underscore) are often things that do not in fact exist at runtime. But you can't impose a blanket rule that private names in stubs should never be considered unexported, because there are things like `os._exit` that are very much public API. The underscore there is to warn users that it's a very dangerous API to use.
> 
> You can however have very high confidence that private names in stubs that define type aliases, protocols, type variables or typed dicts will not in fact exist at runtime. You can also have high confidence that most imported items will not exist at runtime: anything imported that is not included in `__all__`, does not refer to a submodule of the current package, and does not use the redundant alias convention. Lastly, anything decorated with `@type_check_only` in a stub file will not exist at runtime.

And also:

> I do think Carl is correct here that we should not offer `ZQZQ` as an attribute completion here at all if it's `foo.pyi` instead of `foo.py`. In the type checker, if `foo` is defined as a `pyi` file then I don't think we consider `foo` to have a `ZQZQ` attribute at all, because it's been defined in the `foo` module using an import that wasn't an explicit re-export. Stubs often import many things in modules even though they don't exist in those modules at runtime; it's often necessary to do so in order to annotate the public interfaces of those modules.

We should revise our completions to make sure we respect conventions with respect to stub files specifically.

---

_Label `server` added by @BurntSushi on 2025-12-04 15:33_

---

_Label `completions` added by @BurntSushi on 2025-12-04 15:33_

---

_Label `stubs` added by @AlexWaygood on 2025-12-04 15:39_

---

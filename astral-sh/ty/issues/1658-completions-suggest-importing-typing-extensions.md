```yaml
number: 1658
title: "completions suggest importing `typing_extensions` even if you don't depend on it"
type: issue
state: closed
author: Gankra
labels:
  - server
  - completions
assignees: []
created_at: 2025-11-27T18:31:10Z
updated_at: 2025-12-01T16:24:17Z
url: https://github.com/astral-sh/ty/issues/1658
synced_at: 2026-01-12T15:54:25Z
```

# completions suggest importing `typing_extensions` even if you don't depend on it

---

_@Gankra_

* Original discussion https://github.com/astral-sh/ruff/pull/21668#issuecomment-3586965089
* Related to https://github.com/astral-sh/ty/issues/1274

Our vendored version of typeshed includes `typing_extensions`, so we think you can just use import from `typing_extension`s all willy-nilly. This is fine enough in the playground but it will cause issues at runtime if the user actually runs their code and they haven't added it as a dependency of their project.

We need some way to tell the completions subsystem whether typing_extensions is real or not. Alex suggested maybe requiring the typing_extensions module to resolve in `ModuleResolveMode::StubsNotAllowed` mode?

---

_Label `server` added by @AlexWaygood on 2025-11-27 22:23_

---

_Label `completions` added by @AlexWaygood on 2025-11-27 22:23_

---

_Comment by @BurntSushi on 2025-12-01 13:44_

I looked into this, but our current module resolution logic _seems_ to suggest we won't ever resolve `typing_extensions` _unless_ it's in a search path that is considered part of std. Namely:

https://github.com/astral-sh/ruff/blob/5358ddae8857a0450f2a083ecdb7b9d2c0af9981/crates/ty_python_semantic/src/module_resolver/resolver.rs#L687-L699

And:

https://github.com/astral-sh/ruff/blob/5358ddae8857a0450f2a083ecdb7b9d2c0af9981/crates/ty_python_semantic/src/module_resolver/resolver.rs#L714-L721

So I think what this means is that `resolve_real_module("typing_extensions")` will always return nothing? (Unless there is a non-stub `typing_extensions` in a std search path.) But I think this issue is suggesting using this so that, say, it will return a `Module` if there is a `typing_extensions` in an environment?

---

_Comment by @AlexWaygood on 2025-12-01 13:47_

> I looked into this, but our current module resolution logic _seems_ to suggest we won't ever resolve `typing_extensions` _unless_ it's in a search path that is considered part of std.

Ah, yeah, I forgot about that. For the type checker, it's very important that we only ever see the typeshed version of `typing_extensions`, because it's involved in a terrible import cycle with `builtins` and `typing`, so there's a very high likelihood that we'll panic with an opaque error message if we resolve `typing_extensions` to an installed version of the package in `site-packages`. (We probably won't be able to infer the type of `object`, or something similarly awful.) The LSP obviously has different concerns here, though, so maybe we can find a way of disabling that `is_non_shadowable` thing for the LSP...

---

_Comment by @Gankra on 2025-12-01 14:08_

Having uv integration (understanding pyproject.toml) could also help with this I suppose.

---

_Comment by @AlexWaygood on 2025-12-01 14:14_

However we do it, we will eventually need a way to be able to ask "is `typing_extensions` _actually_ available, though?" for https://github.com/astral-sh/ty/issues/1577, too

---

_Closed by @BurntSushi on 2025-12-01 16:24_

---

```yaml
number: 261
title: "Cycle when resolving an imported module with `__all__`"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - imports
assignees: []
created_at: 2025-05-08T03:29:40Z
updated_at: 2025-05-13T14:59:12Z
url: https://github.com/astral-sh/ty/issues/261
synced_at: 2026-01-12T15:54:22Z
```

# Cycle when resolving an imported module with `__all__`

---

_@dhruvmanila_

### Summary

`play.py`:

```py
from foo import bar

# revealed on main: Never
# revealed before `__all__` support: <module 'foo.bar'>
reveal_type(bar)
```

`foo/__init__.pyi`:

```py
from foo import bar

__all__ = ["bar"]
```

`foo/bar/__init__.pyi` (empty file):

```py
```

Running this with:
```sh
cargo run --bin ty -- check --project=/path/to/project
```

This is reproduced with or without the custom typeshed that I've provided below.

Trace logs that are interesting and reveals a cycle on `symbol` query:

```
Resolving import statement from module `foo` into file `/Users/dhruv/playground/ty/isolated3/play.py`
(query) resolve_module{name=foo}
Resolved module `foo` to `/Users/dhruv/playground/ty/isolated3/foo/__init__.pyi`
(query) infer_definition_types{range=48..51, file=File(System("/Users/dhruv/playground/ty/isolated3/play.py"))}
(query) member_lookup_with_policy: <module 'foo'>.bar
imported_symbol: "bar" in /Users/dhruv/playground/ty/isolated3/foo/__init__.pyi
(query) symbol{name="bar", file=/Users/dhruv/playground/ty/isolated3/foo/__init__.pyi}
(query) dunder_all_names{file=System("/Users/dhruv/playground/ty/isolated3/foo/__init__.pyi")}
(query) infer_definition_types{range=16..19, file=File(System("/Users/dhruv/playground/ty/isolated3/foo/__init__.pyi"))}
(query) member_lookup_with_policy: <module 'foo'>.bar
imported_symbol: "bar" in /Users/dhruv/playground/ty/isolated3/foo/__init__.pyi
(query) (CYCLE) symbol{name="bar", file=/Users/dhruv/playground/ty/isolated3/foo/__init__.pyi}
```

This might be related to #113.

The reason this isn't occurring before `__all__` support is that the `bar` module is not being re-exported in `foo/__init__.pyi` via "redundant alias" but is via `__all__`. So, before `__all__` support, it wouldn't trigger the `infer_definition_types` query and fallback to using `Type::ModuleType.to_instance(db).member(db, name)` instead which would avoid the cycle.

<details><summary>Custom typeshed with only the following files:</summary>
<p>

`stdlib/builtins.pyi`:

```py
class object: ...
class type: ...
class int: ...
class str: ...
class list: ...
```

`stdlib/types.pyi`:

```py
__all__ = ["ModuleType"]

class ModuleType: ...
```

`stdlib/typing.pyi`:

```py
__all__ = ["reveal_type"]

def reveal_type[T](obj: T, /) -> T: ...
```

`stdlib/VERSIONS`:

```
builtins: 3.0-
types: 3.0-
typing: 3.5-
```

</p>
</details> 

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-05-08 03:29_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-08 18:14_

---

_Comment by @sharkdp on 2025-05-10 12:50_

Just noting down some things while I am looking into this:

* Due to the "Cycle when …" title, I somehow thought that we would panic here or run into a stack overflow, but we "only" infer an incorrect type here. "Cycle" in the title refers to the fact that we encounter a fixpoint iteration cycle when resolving types here.
* The example *is* self-referential, because `foo/__init__.pyi` contains `from foo import bar`.
* It's true that a `infer_definition_types` cycle is involved in inferring the type *with the provided custom typeshed*, but this is not true for normal typeshed! ~~There is no `WillIterateCycle` salsa event and I can even remove cycle handling from `infer_definition_types`, and the answer is still `Never`.~~ That was wrong. There is still a cycle, but a different one.. on `symbol_by_id`, as the PR description mentions.
* This also doesn't seem `__all__` related to me, as we also infer `Never` if the `__all__` definition is removed.
* I'm also not sure about the "revealed before `__all__` support: <module 'foo.bar'>" comment. I also get `Never` on commit c6f4929cdc596d0b0e0018216fc29b23a8621a92, before `__all__` had been merged?

---

_Comment by @sharkdp on 2025-05-10 13:12_

I'm not really familiar with the module/import resolution part of our codebase, but it seems to me like the underlying problem here is that we need to try and resolve `bar` as a symbol before checking if `bar` is a submodule. The fact that we infer `Never` for a self referential import (in case there is no such submodule) is actually fine, I think? Consider an example like [this](https://play.ty.dev/1dddb2eb-a4fe-471d-a2c0-0f4d587692e0):

`main.py`
```py
from module import x

reveal_type(x)
```

`module.py`
```py
from module import x
```

This will lead to an `ImportError` at runtime, so `Never` is correct in that sense? (ideally, we'd also emit a diagnostic).



So maybe we need to special-case `Never` and treat it as a signal that no such symbol can be found in `module`, and fall back to `KnownClass::ModuleType.to_instance(db).member(db, name)`.


---

_Comment by @sharkdp on 2025-05-10 13:38_

I posted a draft for a fix here: https://github.com/astral-sh/ruff/pull/18005. The ecosystem results look promising. Some false positives go away. There are a lot of new errors, but this can hopefully be attributed to the fact that we previously inferred (a forgiving) `Never` for a lot of things? I didn't have time to look into it yet. Will continue tomorrow.
 

---

_Label `imports` added by @AlexWaygood on 2025-05-10 17:59_

---

_Closed by @sharkdp on 2025-05-13 14:59_

---

```yaml
number: 1195
title: "Cannot resolve imported module `.`"
type: issue
state: closed
author: sharkdp
labels:
  - imports
assignees: []
created_at: 2025-09-17T08:00:57Z
updated_at: 2025-09-25T01:15:37Z
url: https://github.com/astral-sh/ty/issues/1195
synced_at: 2026-01-12T15:54:24Z
```

# Cannot resolve imported module `.`

---

_@sharkdp_

### Summary

When running `ty` on `asynq`, we see errors like:
```
asynq/scheduler.py:23:1: error[unresolved-import] Cannot resolve imported module `.`
asynq/scheduler.py:24:1: error[unresolved-import] Cannot resolve imported module `.`
asynq/scheduler.py:25:1: error[unresolved-import] Cannot resolve imported module `.`
asynq/scheduler.py:26:1: error[unresolved-import] Cannot resolve imported module `.`
asynq/scheduler.py:27:7: error[unresolved-import] Cannot resolve imported module `.async_task`
```

Folder with `scheduler.py` file: [`asynq/`](https://github.com/quora/asynq/tree/master/asynq)

### Version

Current main (ffd650e5fdbd8458065670083ade0316e656b673)

---

_Label `imports` added by @sharkdp on 2025-09-17 08:00_

---

_Comment by @sharkdp on 2025-09-17 08:08_

The same error shows up in this line in `bokeh`, and there are some similar-looking "Cannot resolve imported module `..settings` errors as well:

https://github.com/bokeh/bokeh/blob/62b3e23d53e9a87b6527cf6da5d4e688425f0904/src/bokeh/core/property_aliases.py#L24

---

_Added to milestone `GA` by @carljm on 2025-09-17 15:03_

---

_Comment by @Gankra on 2025-09-18 03:29_

This problem occurs when:

* a module has both a `.py` and a `.pyi`
* the `.py` has a relative import (`from . import ....`)
 
this is because `relative_module_name` invokes `file_to_module(importing_file)` to resolve `.`, but `file_to_module` insists that file <-> module is a bijection, and throws out any answer to the contrary. Since `mymodule.pyi` claims the module name `mymodule`, `file_to_module` believes `mymodule.py` *can't* also resolve to `mymodule`, and so `file_to_module` returns None, and `relative_module_name` fails.

---

_Comment by @Gankra on 2025-09-18 03:39_

You can trivially fix this by changing this condition in `file_to_module` to always return `Some`:

```rs
    if file.path(db) == module_file.path(db) {
        Some(module)
    } else {
        // This path is for a module with the same name but with a different precedence. For example:
        // ```
        // src/foo.py
        // src/foo/__init__.py
        // ```
        // The module name of `src/foo.py` is `foo`, but the module loaded by Python is `src/foo/__init__.py`.
        // That means we need to ignore `src/foo.py` even though it resolves to the same module name.
        None
    }
```

Micha raised the concern of this being possible here, but for whatever reason this is apparently the only situation where it actually happens!

* #869 

---

_Closed by @Gankra on 2025-09-25 01:15_

---

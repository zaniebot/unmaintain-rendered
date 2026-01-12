```yaml
number: 1410
title: Hover for subscript when the argument is a Literal
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-10-22T15:47:02Z
updated_at: 2025-11-14T14:35:45Z
url: https://github.com/astral-sh/ty/issues/1410
synced_at: 2026-01-12T15:54:25Z
```

# Hover for subscript when the argument is a Literal

---

_@MichaReiser_

```py
arg = {
    "something": "else",
    "metadata": {"foo": "bar"},
    "configurable": {"baz": "qux"},
    "callbacks": [],
    "tags": ["tag1", "tag2"],
    "max_concurrency": 1,
    "recursion_limit": 100,
    "run_id": 1,
    "run_name": "test",
}

assert len(arg["callbacks"]) == 1, (
    "ensure_config should not modify the original config"
)
```

ty should show the type of `arg["callback"]` when hovering the subscript expression. 

https://play.ty.dev/e30bc826-2028-4739-8fbb-f70f83d6531d

---

_Label `server` added by @MichaReiser on 2025-10-22 15:47_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:30_

---

_Comment by @Gankra on 2025-11-14 14:02_

Do you mean like, hovering the braces? I think hovering `arg` should show you `arg`. Hovering `"callback"` is arguably not that useful, I guess? But if this was `self.args[self.arg_idx]` I think both the LHS and argument have useful hovers.

---

_Comment by @MichaReiser on 2025-11-14 14:11_

I only just realized that Pylance doesn't support this. I assumed it would because it's something I frequently used in TypeScript:

```ts
const movie = { "name": "test", "year": 2024}

movie["year"]
```

Hovering `["year"]` shows `property "year": number` 

[playground](https://www.typescriptlang.org/play/?#code/FAYw9gdgzgLgBAWzANwJYFM4F44G84BEEAhgugQFyEzqwEA0hAnusQE6VwBMADFwCwBfYMCRp0AbQIt2BALpA)


I deprioritized this to P2, given that Pylance doesn't support it

---

_Renamed from "Hover for subscript" to "Hover for subscript when the argument is a Literal" by @Gankra on 2025-11-14 14:35_

---

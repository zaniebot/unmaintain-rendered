```yaml
number: 1849
title: Improve scope based completions in eager enclosed scopes
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-11T02:04:35Z
updated_at: 2025-12-11T17:42:35Z
url: https://github.com/astral-sh/ty/issues/1849
synced_at: 2026-01-12T15:54:25Z
```

# Improve scope based completions in eager enclosed scopes

---

_@BurntSushi_

Specifically, [from @carljm](https://github.com/astral-sh/ruff/pull/21872#issuecomment-3639670356):

> For eager enclosed scopes (scopes that are executed immediately at the point of definition, like class bodies or comprehensions) we do better in type inference, because in that case we do know precisely where in the enclosing scope the nested scope executed. (If you wanted, I think you could use those same "eager nested scope snapshots" from the semantic index for more precision here as well, but I think that could be a separate follow-up change.)

Ref https://github.com/astral-sh/ruff/pull/21872

Ref https://github.com/astral-sh/ty/issues/1294

---

_Label `server` added by @BurntSushi on 2025-12-11 02:04_

---

_Label `completions` added by @BurntSushi on 2025-12-11 02:04_

---

_Comment by @BurntSushi on 2025-12-11 13:27_

Actually, I wonder if this is already happening? e.g., See:

https://github.com/astral-sh/ruff/blob/8647844572e405c241a96dbc37e51cfa6b6b69a9/crates/ty_python_semantic/src/types/list_members.rs#L325

And:

https://github.com/astral-sh/ruff/blob/8647844572e405c241a96dbc37e51cfa6b6b69a9/crates/ty_python_semantic/src/types/list_members.rs#L417-L426

And:

https://github.com/astral-sh/ruff/blob/8647844572e405c241a96dbc37e51cfa6b6b69a9/crates/ty_python_semantic/src/types/list_members.rs#L465-L474

---

_Comment by @carljm on 2025-12-11 17:42_

@BurntSushi none of those links look like they are using the eager-nested-scope live-defs snapshots. They are all using end-of-scope, which is probably right for looking up members of a class or module, but generally not right for walking enclosed scopes looking for definitions of a name.

---

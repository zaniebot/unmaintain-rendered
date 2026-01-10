```yaml
number: 11907
title: "[red-knot]: Normalize paths stored in `Vfs`"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2024-06-17T16:03:05Z
updated_at: 2024-07-09T07:52:14Z
url: https://github.com/astral-sh/ruff/issues/11907
synced_at: 2026-01-10T11:09:53Z
```

# [red-knot]: Normalize paths stored in `Vfs`

---

_Issue opened by @MichaReiser on 2024-06-17 16:03_

I'm not entirely sure where the normalization should happen, but I ran into the following bug:

```rust
db.memory_file_system().write_files([
      ("src/a.py", "from foo import x"),
      ("src/foo.py", "x = 10\ndef foo(): ..."),
])?;

 let x_ty = public_symbol_ty(&db, a, "x").unwrap();

db.memory_file_system()
      .write_file("src/foo.py", "x = 20\ndef foo(): ...")?;

let foo = system_path_to_file(&db, "src/foo.py").unwrap();
foo.touch(&mut db);

let x_ty_2 = public_symbol_ty(&db, a, "x").unwrap();
```

The code updates `foo.py` after its initial write. What I would have expected is that changing foo, retriggers the type inference for `a.py`, but it didn't. 

The problem is simple. The module resolver resolves `from foo import x` to the path `/src/foo.py` but we call `system_path_to_file` with `src/foo.py` (note the missing leading `/`. 

I think the solution here is that `Vfs` should normalize paths (not canoncialize, just normalize them) to ensure the path entries are unique.

For now, I'm going to fix up the test to use absolute paths.

---

_Label `red-knot` added by @MichaReiser on 2024-06-17 16:03_

---

_Renamed from "[red-knot]: Normalize paths" to "[red-knot]: Normalize paths stored in `Vfs`" by @MichaReiser on 2024-06-17 16:03_

---

_Closed by @MichaReiser on 2024-07-09 07:52_

---

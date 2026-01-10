```yaml
number: 1526
title: "`import whatever.thispackage.a.b` in an `__init__.py` should introduce the submodule attribute `a`"
type: issue
state: open
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-11T20:36:49Z
updated_at: 2025-11-11T20:37:05Z
url: https://github.com/astral-sh/ty/issues/1526
synced_at: 2026-01-10T02:06:25Z
```

# `import whatever.thispackage.a.b` in an `__init__.py` should introduce the submodule attribute `a`

---

_Issue opened by @Gankra on 2025-11-11 20:36_

We do this for `from whatever.thispackage.a.b import c` but not the equivalent `import`.

[Nonstandard-imports contains a test for this](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md#import-of-nested-submodule-in-__init__)

This is just a random inconsistency because `ImportFromSubmoduleDefinitionNodeRef` is *only* for `from...import`.

The fix would be to introduce an equivalent of this logic:

https://github.com/astral-sh/ruff/blob/e4374f14ed12873541d9a9ccca7eea92456684f6/crates/ty_python_semantic/src/semantic_index/builder.rs#L1452-L1497

To here:

https://github.com/astral-sh/ruff/blob/e4374f14ed12873541d9a9ccca7eea92456684f6/crates/ty_python_semantic/src/semantic_index/builder.rs#L1420-L1422

And then you need to either:

* change `ImportFromSubmoduleDefinitionNode(Ref)` to take either an `ast::StmtImportFrom` *or* an `ast::StmtImport` (make an `AnyImport` enum?)
* introduce `ImportSubmoduleDefinitionNode(Ref)` that just copy-pastes all the logic

The enum approach is probably the better one.

---

_Label `imports` added by @Gankra on 2025-11-11 20:36_

---

_Comment by @Gankra on 2025-11-11 20:37_

I don't have any plans to do this, but this might be a good first issue?

---

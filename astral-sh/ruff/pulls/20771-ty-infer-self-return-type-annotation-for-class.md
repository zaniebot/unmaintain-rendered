```yaml
number: 20771
title: "[ty] infer `Self` return type annotation for class methods"
type: pull_request
state: closed
author: Glyphack
labels:
  - ty
assignees: []
draft: true
base: main
head: function-argument-cls
created_at: 2025-10-08T16:26:47Z
updated_at: 2025-12-09T18:56:59Z
url: https://github.com/astral-sh/ruff/pull/20771
synced_at: 2026-01-12T15:57:09Z
```

# [ty] infer `Self` return type annotation for class methods

---

_@Glyphack_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Infer annotated `Self` return type correctly for class methods.

Example:

```py
from typing import Type, Self

class C:
    @classmethod
    def make_instance(cls: Type[Self]) -> Self:
        return cls()

reveal_type(C.make_instance())  # Should be C but is Unknown
reveal_type(C.make_instance)  # revealed: bound method <class 'C'>.make_instance() -> C
```

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "`Self` return type annotation is inferred as unknown for class methods" to "[ty] infer `Self` return type annotation for class methods" by @Glyphack on 2025-10-08 16:27_

---

_Label `ty` added by @AlexWaygood on 2025-10-08 17:17_

---

_Comment by @github-actions[bot] on 2025-10-25 10:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-25 10:56:01.580589318 +0000
+++ new-output.txt	2025-10-25 10:56:33.722533918 +0000
@@ -1,3 +1,3117901 @@
+DEBUGPRINT[68]: types.rs:5987: specialization=Specialization {
+    generic_context: GenericContext {
+        variables_inner: {
+            BoundTypeVarIdentity {
+                identity: TypeVarIdentity {
+                    name: Name("T_co"),
+                    definition: Some(
+                        Definition {
+                            [salsa id]: Id(1407),
+                            file: File {
+                                path: System(
+                                    "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                ),
+                                status: Exists,
+                                permissions: Some(
+                                    33188,
+                                ),
+                                revision: FileRevision(
+                                    32491904714820139829054224747,
+                                ),
+                            },
+                            file_scope: FileScopeId(
+                                0,
+                            ),
+                            place: Symbol(
+                                ScopedSymbolId(
+                                    7,
+                                ),
+                            ),
+                            kind: Assignment(
+                                AssignmentDefinitionKind {
+                                    target_kind: Single,
+                                    value: AstNodeRef {
+                                        kind: ExprCall,
+                                        range: 572..603,
+                                    },
+                                    target: AstNodeRef {
+                                        kind: ExprName,
+                                        range: 565..569,
+                                    },
+                                },
+                            ),
+                            is_reexported: true,
+                        },
+                    ),
+                    kind: Legacy,
+                },
+                binding_context: Definition(
+                    Definition {
+                        [salsa id]: Id(140e),
+                        file: File {
+                            path: System(
+                                "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                            ),
+                            status: Exists,
+                            permissions: Some(
+                                33188,
+                            ),
+                            revision: FileRevision(
+                                32491904714820139829054224747,
+                            ),
+                        },
+                        file_scope: FileScopeId(
+                            0,
+                        ),
+                        place: Symbol(
+                            ScopedSymbolId(
+                                9,
+                            ),
+                        ),
+                        kind: Class(
+                            AstNodeRef {
+                                kind: StmtClassDef,
+                                range: 657..814,
+                            },
+                        ),
+                        is_reexported: true,
+                    },
+                ),
+            }: GenericContextTypeVar {
+                bound_typevar: BoundTypeVarInstance {
+                    typevar: TypeVarInstance {
+                        identity: TypeVarIdentity {
+                            name: Name("T_co"),
+                            definition: Some(
+                                Definition {
+                                    [salsa id]: Id(1407),
+                                    file: File {
+                                        path: System(
+                                            "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                        ),
+                                        status: Exists,
+                                        permissions: Some(
+                                            33188,
+                                        ),
+                                        revision: FileRevision(
+                                            32491904714820139829054224747,
+                                        ),
+                                    },
+                                    file_scope: FileScopeId(
+                                        0,
+                                    ),
+                                    place: Symbol(
+                                        ScopedSymbolId(
+                                            7,
+                                        ),
+                                    ),
+                                    kind: Assignment(
+                                        AssignmentDefinitionKind {
+                                            target_kind: Single,
+                                            value: AstNodeRef {
+                                                kind: ExprCall,
+                                                range: 572..603,
+                                            },
+                                            target: AstNodeRef {
+                                                kind: ExprName,
+                                                range: 565..569,
+                                            },
+                                        },
+                                    ),
+                                    is_reexported: true,
+                                },
+                            ),
+                            kind: Legacy,
+                        },
+                        _bound_or_constraints: None,
+                        explicit_variance: Some(
+                            Covariant,
+                        ),
+                        _default: None,
+                    },
+                    binding_context: Definition(
+                        Definition {
+                            [salsa id]: Id(140e),
+                            file: File {
+                                path: System(
+                                    "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                ),
+                                status: Exists,
+                                permissions: Some(
+                                    33188,
+                                ),
+                                revision: FileRevision(
+                                    32491904714820139829054224747,
+                                ),
+                            },
+                            file_scope: FileScopeId(
+                                0,
+                            ),
+                            place: Symbol(
+                                ScopedSymbolId(
+                                    9,
+                                ),
+                            ),
+                            kind: Class(
+                                AstNodeRef {
+                                    kind: StmtClassDef,
+                                    range: 657..814,
+                                },
+                            ),
+                            is_reexported: true,
+                        },
+                    ),
+                },
+                should_promote_literals: false,
+            },
+        },
+    },
+    types: [
+        TypeVar(
+            BoundTypeVarInstance {
+                typevar: TypeVarInstance {
+                    identity: TypeVarIdentity {
+                        name: Name("T_co"),
+                        definition: Some(
+                            Definition {
+                                [salsa id]: Id(1407),
+                                file: File {
+                                    path: System(
+                                        "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                    ),
+                                    status: Exists,
+                                    permissions: Some(
+                                        33188,
+                                    ),
+                                    revision: FileRevision(
+                                        32491904714820139829054224747,
+                                    ),
+                                },
+                                file_scope: FileScopeId(
+                                    0,
+                                ),
+                                place: Symbol(
+                                    ScopedSymbolId(
+                                        7,
+                                    ),
+                                ),
+                                kind: Assignment(
+                                    AssignmentDefinitionKind {
+                                        target_kind: Single,
+                                        value: AstNodeRef {
+                                            kind: ExprCall,
+                                            range: 572..603,
+                                        },
+                                        target: AstNodeRef {
+                                            kind: ExprName,
+                                            range: 565..569,
+                                        },
+                                    },
+                                ),
+                                is_reexported: true,
+                            },
+                        ),
+                        kind: Legacy,
+                    },
+                    _bound_or_constraints: None,
+                    explicit_variance: Some(
+                        Covariant,
+                    ),
+                    _default: None,
+                },
+                binding_context: Definition(
+                    Definition {
+                        [salsa id]: Id(140e),
+                        file: File {
+                            path: System(
+                                "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                            ),
+                            status: Exists,
+                            permissions: Some(
+                                33188,
+                            ),
+                            revision: FileRevision(
+                                32491904714820139829054224747,
+                            ),
+                        },
+                        file_scope: FileScopeId(
+                            0,
+                        ),
+                        place: Symbol(
+                            ScopedSymbolId(
+                                9,
+                            ),
+                        ),
+                        kind: Class(
+                            AstNodeRef {
+                                kind: StmtClassDef,
+                                range: 657..814,
+                            },
+                        ),
+                        is_reexported: true,
+                    },
+                ),
+            },
+        ),
+    ],
+    materialization_kind: None,
+    tuple_inner: None,
+}
+DEBUGPRINT[67]: types.rs:5987: self=def __init__[T_co](self, items: Iterable[T_co@ImmutableList]) -> None
+DEBUGPRINT[65]: types.rs:5986: new_specialization=FunctionLiteral(
+    FunctionType {
+        literal: FunctionLiteral {
+            last_definition: OverloadLiteral {
+                name: Name("__init__"),
+                known: None,
+                body_scope: ScopeId {
+                    [salsa id]: Id(1002),
+                    file: File {
+                        path: System(
+                            "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                        ),
+                        status: Exists,
+                        permissions: Some(
+                            33188,
+                        ),
+                        revision: FileRevision(
+                            32491904714820139829054224747,
+                        ),
+                    },
+                    file_scope_id: FileScopeId(
+                        2,
+                    ),
+                },
+                decorators: FunctionDecorators(
+                    0x0,
+                ),
+                deprecated: None,
+                dataclass_transformer_params: None,
+            },
+        },
+        updated_signature: Some(
+            CallableSignature {
+                overloads: [
+                    Signature {
+                        generic_context: Some(
+                            GenericContext {
+                                variables_inner: {
+                                    BoundTypeVarIdentity {
+                                        identity: TypeVarIdentity {
+                                            name: Name("Self"),
+                                            definition: Some(
+                                                Definition {
+                                                    [salsa id]: Id(140e),
+                                                    file: File {
+                                                        path: System(
+                                                            "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                        ),
+                                                        status: Exists,
+                                                        permissions: Some(
+                                                            33188,
+                                                        ),
+                                                        revision: FileRevision(
+                                                            32491904714820139829054224747,
+                                                        ),
+                                                    },
+                                                    file_scope: FileScopeId(
+                                                        0,
+                                                    ),
+                                                    place: Symbol(
+                                                        ScopedSymbolId(
+                                                            9,
+                                                        ),
+                                                    ),
+                                                    kind: Class(
+                                                        AstNodeRef {
+                                                            kind: StmtClassDef,
+                                                            range: 657..814,
+                                                        },
+                                                    ),
+                                                    is_reexported: true,
+                                                },
+                                            ),
+                                            kind: TypingSelf,
+                                        },
+                                        binding_context: Definition(
+                                            Definition {
+                                                [salsa id]: Id(140b),
+                                                file: File {
+                                                    path: System(
+                                                        "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                    ),
+                                                    status: Exists,
+                                                    permissions: Some(
+                                                        33188,
+                                                    ),
+                                                    revision: FileRevision(
+                                                        32491904714820139829054224747,
+                                                    ),
+                                                },
+                                                file_scope: FileScopeId(
+                                                    1,
+                                                ),
+                                                place: Symbol(
+                                                    ScopedSymbolId(
+                                                        2,
+                                                    ),
+                                                ),
+                                                kind: Function(
+                                                    AstNodeRef {
+                                                        kind: StmtFunctionDef,
+                                                        range: 697..759,
+                                                    },
+                                                ),
+                                                is_reexported: true,
+                                            },
+                                        ),
+                                    }: GenericContextTypeVar {
+                                        bound_typevar: BoundTypeVarInstance {
+                                            typevar: TypeVarInstance {
+                                                identity: TypeVarIdentity {
+                                                    name: Name("Self"),
+                                                    definition: Some(
+                                                        Definition {
+                                                            [salsa id]: Id(140e),
+                                                            file: File {
+                                                                path: System(
+                                                                    "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                ),
+                                                                status: Exists,
+                                                                permissions: Some(
+                                                                    33188,
+                                                                ),
+                                                                revision: FileRevision(
+                                                                    32491904714820139829054224747,
+                                                                ),
+                                                            },
+                                                            file_scope: FileScopeId(
+                                                                0,
+                                                            ),
+                                                            place: Symbol(
+                                                                ScopedSymbolId(
+                                                                    9,
+                                                                ),
+                                                            ),
+                                                            kind: Class(
+                                                                AstNodeRef {
+                                                                    kind: StmtClassDef,
+                                                                    range: 657..814,
+                                                                },
+                                                            ),
+                                                            is_reexported: true,
+                                                        },
+                                                    ),
+                                                    kind: TypingSelf,
+                                                },
+                                                _bound_or_constraints: Some(
+                                                    Eager(
+                                                        UpperBound(
+                                                            NominalInstance(
+                                                                NominalInstanceType(
+                                                                    NonTuple(
+                                                                        Generic(
+                                                                            GenericAlias {
+                                                                                origin: ClassLiteral {
+                                                                                    name: Name("ImmutableList"),
+                                                                                    body_scope: ScopeId {
+                                                                                        [salsa id]: Id(1001),
+                                                                                        file: File {
+                                                                                            path: System(
+                                                                                                "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                            ),
+                                                                                            status: Exists,
+                                                                                            permissions: Some(
+                                                                                                33188,
+                                                                                            ),
+                                                                                            revision: FileRevision(
+                                                                                                32491904714820139829054224747,
+                                                                                            ),
+                                                                                        },
+                                                                                        file_scope_id: FileScopeId(
+                                                                                            1,
+                                                                                        ),
+                                                                                    },
+                                                                                    known: None,
+                                                                                    deprecated: None,
+                                                                                    type_check_only: false,
+                                                                                    dataclass_params: None,
+                                                                                    dataclass_transformer_params: None,
+                                                                                },
+                                                                                specialization: Specialization {
+                                                                                    generic_context: GenericContext {
+                                                                                        variables_inner: {
+                                                                                            BoundTypeVarIdentity {
+                                                                                                identity: TypeVarIdentity {
+                                                                                                    name: Name("T_co"),
+                                                                                                    definition: Some(
+                                                                                                        Definition {
+                                                                                                            [salsa id]: Id(1407),
+                                                                                                            file: File {
+                                                                                                                path: System(
+                                                                                                                    "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                                                ),
+                                                                                                                status: Exists,
+                                                                                                                permissions: Some(
+                                                                                                                    33188,
+                                                                                                                ),
+                                                                                                                revision: FileRevision(
+                                                                                                                    32491904714820139829054224747,
+                                                                                                                ),
+                                                                                                            },
+                                                                                                            file_scope: FileScopeId(
+                                                                                                                0,
+                                                                                                            ),
+                                                                                                            place: Symbol(
+                                                                                                                ScopedSymbolId(
+                                                                                                                    7,
+                                                                                                                ),
+                                                                                                            ),
+                                                                                                            kind: Assignment(
+                                                                                                                AssignmentDefinitionKind {
+                                                                                                                    target_kind: Single,
+                                                                                                                    value: AstNodeRef {
+                                                                                                                        kind: ExprCall,
+                                                                                                                        range: 572..603,
+                                                                                                                    },
+                                                                                                                    target: AstNodeRef {
+                                                                                                                        kind: ExprName,
+                                                                                                                        range: 565..569,
+                                                                                                                    },
+                                                                                                                },
+                                                                                                            ),
+                                                                                                            is_reexported: true,
+                                                                                                        },
+                                                                                                    ),
+                                                                                                    kind: Legacy,
+                                                                                                },
+                                                                                                binding_context: Definition(
+                                                                                                    Definition {
+                                                                                                        [salsa id]: Id(140e),
+                                                                                                        file: File {
+                                                                                                            path: System(
+                                                                                                                "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                                            ),
+                                                                                                            status: Exists,
+                                                                                                            permissions: Some(
+                                                                                                                33188,
+                                                                                                            ),
+                                                                                                            revision: FileRevision(
+                                                                                                                32491904714820139829054224747,
+                                                                                                            ),
+                                                                                                        },
+                                                                                                        file_scope: FileScopeId(
+                                                                                                            0,
+                                                                                                        ),
+                                                                                                        place: Symbol(
+                                                                                                            ScopedSymbolId(
+                                                                                                                9,
+                                                                                                            ),
+                                                                                                        ),
+                                                                                                        kind: Class(
+                                                                                                            AstNodeRef {
+                                                                                                                kind: StmtClassDef,
+                                                                                                                range: 657..814,
+                                                                                                            },
+                                                                                                        ),
+                                                                                                        is_reexported: true,
+                                                                                                    },
+                                                                                                ),
+                                                                                            }: GenericContextTypeVar {
+                                                                                                bound_typevar: BoundTypeVarInstance {
+                                                                                                    typevar: TypeVarInstance {
+                                                                                                        identity: TypeVarIdentity {
+                                                                                                            name: Name("T_co"),
+                                                                                                            definition: Some(
+                                                                                                                Definition {
+                                                                                                                    [salsa id]: Id(1407),
+                                                                                                                    file: File {
+                                                                                                                        path: System(
+                                                                                                                            "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                                                        ),
+                                                                                                                        status: Exists,
+                                                                                                                        permissions: Some(
+                                                                                                                            33188,
+                                                                                                                        ),
+                                                                                                                        revision: FileRevision(
+                                                                                                                            32491904714820139829054224747,
+                                                                                                                        ),
+                                                                                                                    },
+                                                                                                                    file_scope: FileScopeId(
+                                                                                                                        0,
+                                                                                                                    ),
+                                                                                                                    place: Symbol(
+                                                                                                                        ScopedSymbolId(
+                                                                                                                            7,
+                                                                                                                        ),
+                                                                                                                    ),
+                                                                                                                    kind: Assignment(
+                                                                                                                        AssignmentDefinitionKind {
+                                                                                                                            target_kind: Single,
+                                                                                                                            value: AstNodeRef {
+                                                                                                                                kind: ExprCall,
+                                                                                                                                range: 572..603,
+                                                                                                                            },
+                                                                                                                            target: AstNodeRef {
+                                                                                                                                kind: ExprName,
+                                                                                                                                range: 565..569,
+                                                                                                                            },
+                                                                                                                        },
+                                                                                                                    ),
+                                                                                                                    is_reexported: true,
+                                                                                                                },
+                                                                                                            ),
+                                                                                                            kind: Legacy,
+                                                                                                        },
+                                                                                                        _bound_or_constraints: None,
+                                                                                                        explicit_variance: Some(
+                                                                                                            Covariant,
+                                                                                                        ),
+                                                                                                        _default: None,
+                                                                                                    },
+                                                                                                    binding_context: Definition(
+                                                                                                        Definition {
+                                                                                                            [salsa id]: Id(140e),
+                                                                                                            file: File {
+                                                                                                                path: System(
+                                                                                                                    "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                                                ),
+                                                                                                                status: Exists,
+                                                                                                                permissions: Some(
+                                                                                                                    33188,
+                                                                                                                ),
+                                                                                                                revision: FileRevision(
+                                                                                                                    32491904714820139829054224747,
+                                                                                                                ),
+                                                                                                            },
+                                                                                                            file_scope: FileScopeId(
+                                                                                                                0,
+                                                                                                            ),
+                                                                                                            place: Symbol(
+                                                                                                                ScopedSymbolId(
+                                                                                                                    9,
+                                                                                                                ),
+                                                                                                            ),
+                                                                                                            kind: Class(
+                                                                                                                AstNodeRef {
+                                                                                                                    kind: StmtClassDef,
+                                                                                                                    range: 657..814,
+                                                                                                                },
+                                                                                                            ),
+                                                                                                            is_reexported: true,
+                                                                                                        },
+                                                                                                    ),
+                                                                                                },
+                                                                                                should_promote_literals: false,
+                                                                                            },
+                                                                                        },
+                                                                                    },
+                                                                                    types: [
+                                                                                        TypeVar(
+                                                                                            BoundTypeVarInstance {
+                                                                                                typevar: TypeVarInstance {
+                                                                                                    identity: TypeVarIdentity {
+                                                                                                        name: Name("T_co"),
+                                                                                                        definition: Some(
+                                                                                                            Definition {
+                                                                                                                [salsa id]: Id(1407),
+                                                                                                                file: File {
+                                                                                                                    path: System(
+                                                                                                                        "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                                                    ),
+                                                                                                                    status: Exists,
+                                                                                                                    permissions: Some(
+                                                                                                                        33188,
+                                                                                                                    ),
+                                                                                                                    revision: FileRevision(
+                                                                                                                        32491904714820139829054224747,
+                                                                                                                    ),
+                                                                                                                },
+                                                                                                                file_scope: FileScopeId(
+                                                                                                                    0,
+                                                                                                                ),
+                                                                                                                place: Symbol(
+                                                                                                                    ScopedSymbolId(
+                                                                                                                        7,
+                                                                                                                    ),
+                                                                                                                ),
+                                                                                                                kind: Assignment(
+                                                                                                                    AssignmentDefinitionKind {
+                                                                                                                        target_kind: Single,
+                                                                                                                        value: AstNodeRef {
+                                                                                                                            kind: ExprCall,
+                                                                                                                            range: 572..603,
+                                                                                                                        },
+                                                                                                                        target: AstNodeRef {
+                                                                                                                            kind: ExprName,
+                                                                                                                            range: 565..569,
+                                                                                                                        },
+                                                                                                                    },
+                                                                                                                ),
+                                                                                                                is_reexported: true,
+                                                                                                            },
+                                                                                                        ),
+                                                                                                        kind: Legacy,
+                                                                                                    },
+                                                                                                    _bound_or_constraints: None,
+                                                                                                    explicit_variance: Some(
+                                                                                                        Covariant,
+                                                                                                    ),
+                                                                                                    _default: None,
+                                                                                                },
+                                                                                                binding_context: Definition(
+                                                                                                    Definition {
+                                                                                                        [salsa id]: Id(140e),
+                                                                                                        file: File {
+                                                                                                            path: System(
+                                                                                                                "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                                                                            ),
+                                                                                                            status: Exists,
+                                                                                                            permissions: Some(
+                                                                                                                33188,
+                                                                                                            ),
+                                                                                                            revision: FileRevision(
+                                                                                                                32491904714820139829054224747,
+                                                                                                            ),
+                                                                                                        },
+                                                                                                        file_scope: FileScopeId(
+                                                                                                            0,
+                                                                                                        ),
+                                                                                                        place: Symbol(
+                                                                                                            ScopedSymbolId(
+                                                                                                                9,
+                                                                                                            ),
+                                                                                                        ),
+                                                                                                        kind: Class(
+                                                                                                            AstNodeRef {
+                                                                                                                kind: StmtClassDef,
+                                                                                                                range: 657..814,
+                                                                                                            },
+                                                                                                        ),
+                                                                                                        is_reexported: true,
+                                                                                                    },
+                                                                                                ),
+                                                                                            },
+                                                                                        ),
+                                                                                    ],
+                                                                                    materialization_kind: None,
+                                                                                    tuple_inner: None,
+                                                                                },
+                                                                            },
+                                                                        ),
+                                                                    ),
+                                                                ),
+                                                            ),
+                                                        ),
+                                                    ),
+                                                ),
+                                                explicit_variance: Some(
+                                                    Invariant,
+                                                ),
+                                                _default: None,
+                                            },
+                                            binding_context: Definition(
+                                                Definition {
+                                                    [salsa id]: Id(140b),
+                                                    file: File {
+                                                        path: System(
+                                                            "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                        ),
+                                                        status: Exists,
+                                                        permissions: Some(
+                                                            33188,
+                                                        ),
+                                                        revision: FileRevision(
+                                                            32491904714820139829054224747,
+                                                        ),
+                                                    },
+                                                    file_scope: FileScopeId(
+                                                        1,
+                                                    ),
+                                                    place: Symbol(
+                                                        ScopedSymbolId(
+                                                            2,
+                                                        ),
+                                                    ),
+                                                    kind: Function(
+                                                        AstNodeRef {
+                                                            kind: StmtFunctionDef,
+                                                            range: 697..759,
+                                                        },
+                                                    ),
+                                                    is_reexported: true,
+                                                },
+                                            ),
+                                        },
+                                        should_promote_literals: false,
+                                    },
+                                    BoundTypeVarIdentity {
+                                        identity: TypeVarIdentity {
+                                            name: Name("T_co"),
+                                            definition: Some(
+                                                Definition {
+                                                    [salsa id]: Id(1407),
+                                                    file: File {
+                                                        path: System(
+                                                            "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                        ),
+                                                        status: Exists,
+                                                        permissions: Some(
+                                                            33188,
+                                                        ),
+                                                        revision: FileRevision(
+                                                            32491904714820139829054224747,
+                                                        ),
+                                                    },
+                                                    file_scope: FileScopeId(
+                                                        0,
+                                                    ),
+                                                    place: Symbol(
+                                                        ScopedSymbolId(
+                                                            7,
+                                                        ),
+                                                    ),
+                                                    kind: Assignment(
+                                                        AssignmentDefinitionKind {
+                                                            target_kind: Single,
+                                                            value: AstNodeRef {
+                                                                kind: ExprCall,
+                                                                range: 572..603,
+                                                            },
+                                                            target: AstNodeRef {
+                                                                kind: ExprName,
+                                                                range: 565..569,
+                                                            },
+                                                        },
+                                                    ),
+                                                    is_reexported: true,
+                                                },
+                                            ),
+                                            kind: Legacy,
+                                        },
+                                        binding_context: Definition(
+                                            Definition {
+                                                [salsa id]: Id(140e),
+                                                file: File {
+                                                    path: System(
+                                                        "/home/runner/work/ruff/ruff/typing/conformance/tests/generics_variance.py",
+                                                    ),
+                                                    status: Exists,
+                                                    permissions: Some(
+                                                        33188,
+                                                    ),
+                                                    revision: FileRevision(
+                                                        32491904714820139829054224747,
+                                                    ),
+                                                },
+                                                file_scope: FileScopeId(
+                                                    0,
+                                                ),
+                                                place: Symbol(
+                                                    ScopedSymbolId(
+                                                        9,
+                                                    ),
+                                                ),
+                                                kind: Class(
+                                                    AstNodeRef {
+                                                        kind: StmtClassDef,
+                                                        range: 657..814,
+                                                    },
+                                                ),
+                                                is_reexported: true,
+                                            },
+                                        ),
+                                    }: GenericContextTypeVar {
+                                        bound_typevar: BoundTypeVarInstance {
+                                            typevar: TypeVarInstance {
+                                                identity: TypeVarIdentity {
+                                                    name: Name("T_co"),
+                                       ...*[Comment body truncated]*

---

_Comment by @Glyphack on 2025-10-25 10:58_

I thought this was a separate issue than not supporting `type[Self]` but after playing around with it I realized that's the root cause.
I think this playground link shows how:
https://play.ty.dev/55f8e04b-148c-4ea0-8916-e302591678bb

Maybe this PR I can only add the generic context when encountering `type[Self]`

---

_Comment by @Glyphack on 2025-12-09 18:56_

This is fixed already.

---

_Closed by @Glyphack on 2025-12-09 18:56_

---

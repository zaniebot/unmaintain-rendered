```yaml
number: 19820
title: "[ty] add nested bindings to the semantic index"
type: pull_request
state: open
author: oconnor663
labels:
  - ty
assignees: []
draft: true
base: main
head: jack/semantic-index-nested-bindings
created_at: 2025-08-08T03:40:01Z
updated_at: 2025-10-29T15:05:28Z
url: https://github.com/astral-sh/ruff/pull/19820
synced_at: 2026-01-10T16:59:49Z
```

# [ty] add nested bindings to the semantic index

---

_Pull request opened by @oconnor663 on 2025-08-08 03:40_

This PR replaces #19703. It has two main components:

1. Add free variable tracking to the `SemanticIndexBuilder`, and resolve free variables in `pop_scope`. This lets us determine which variables resolve where in the semantic index building pass, without requiring a second pass and without requiring us to defer function bodies. Previously we were doing some of this in `infer.rs` as a poor man's second pass, particularly `infer_nonlocal`, which used to flag invalid `nonlocal` statements. That's now a semantic syntax error in the builder, and `infer_nonlocal` is deleted. In my first pass at #19703 I was recording several new types of metadata in the index itself, including all nonlocal resolutions and nested references. But based on feedback from Micha and Carl, I've pared that down substantially to avoid wasting space. In this PR the only new metadata in the semantic index (specifically in the symbol tables) is `nested_scopes_with_bindings`, which should be empty for most scopes. (The new free variables collection lives in the builder and isn't persisted in the index.)
2. Now that `nested_scopes_with_bindings` is available, take advantage of it to consider all nested bindings when inferring the "public" type of a variable (i.e. its type as viewed from other scopes or other modules). This is done in `place_by_id`. Previously `infer_place_load` (which calls `place` -> `place_by_id`) was computing a union of all the local inferred types it encountered when walking enclosing scopes, but that was missing bindings in sibling/cousin scopes. Now we see those bindings, and the union builder in `infer_place_load` is deleted.

Examples:

```py
def _():
    x = 1
    def _():
        nonlocal x
        x = 2
        def _():
            nonlocal x
            x = 3
    def _():
        # Previously: Literal[1]
        # Now: Literal[1, 2, 3, 4]
        reveal_type(x)
        def _():
            nonlocal x
            x = 4

y = 1
def f():
    global y
    y = 2
def g():
    # Previously: Unknown | Literal[1]
    # Now: Unknown | Literal[1, 2]
    reveal_type(y)
```

Now that we can see nested bindings of globals, I'm going to put up a [follow-up PR on top of this one](https://github.com/astral-sh/ruff/pull/19821) that deletes `infer_global` and allows `global` statements that don't match an explicit symbol in the module scope. I don't know if we'll want that change, so I'm keeping it out of this PR.

There's still a lot of scope walking in `infer.rs`. The type unioning is deleted from `infer_place_load`, but the scope walk is still there to find the defining scope and consider snapshots on the way. `add_binding` also still does a scope walk to find the relevant declaration. We could consider storing these resolutions in the semantic index, but even if we don't, it might be nice to define one function like `resolve_symbol` that all these callers (plus `ide_support.rs`) can use. This PR doesn't cover that.

---

_Review requested from @carljm by @oconnor663 on 2025-08-08 03:40_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-08-08 03:40_

---

_Review requested from @sharkdp by @oconnor663 on 2025-08-08 03:40_

---

_Review requested from @dcreager by @oconnor663 on 2025-08-08 03:40_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-08-08 03:40_

---

_Review requested from @dhruvmanila by @oconnor663 on 2025-08-08 03:40_

---

_Comment by @github-actions[bot] on 2025-08-08 03:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 03:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
operator (https://github.com/canonical/operator)
- ops/lib/__init__.py:86:16: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | None` is possibly unbound
+ ops/lib/__init__.py:86:16: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | dict[Unknown, Unknown] | None` is possibly unbound

cloud-init (https://github.com/canonical/cloud-init)
- tools/mock-meta.py:352:26: warning[possibly-unbound-attribute] Attribute `get_data` on type `Unknown | None` is possibly unbound
+ tools/mock-meta.py:352:26: warning[possibly-unbound-attribute] Attribute `get_data` on type `Unknown | UserDataHandler | None` is possibly unbound
- tools/mock-meta.py:353:26: warning[possibly-unbound-attribute] Attribute `get_data` on type `Unknown | None` is possibly unbound
+ tools/mock-meta.py:353:26: warning[possibly-unbound-attribute] Attribute `get_data` on type `Unknown | MetaDataHandler | None` is possibly unbound

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/arch.py:231:21: warning[possibly-unbound-attribute] Attribute `cpsr` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/arch.py:231:21: warning[possibly-unbound-attribute] Attribute `cpsr` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/arch.py:232:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pwndbg/aglib/disasm/disassembly.py:69:8: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:69:8: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:79:5: error[no-matching-overload] No overload of bound method `pop` matches arguments
- pwndbg/aglib/disasm/disassembly.py:79:36: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:79:36: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/disasm/disassembly.py:174:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:174:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:176:37: error[invalid-argument-type] Argument to function `peek` is incorrect: Expected `int`, found `(Unknown & ~None) | Unknown | int | None`
- pwndbg/aglib/disasm/disassembly.py:199:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:199:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:201:37: error[invalid-argument-type] Argument to function `peek` is incorrect: Expected `int`, found `(Unknown & ~None) | Unknown | int | None`
- pwndbg/aglib/disasm/disassembly.py:276:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:276:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/disasm/disassembly.py:283:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:283:19: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/disasm/disassembly.py:296:9: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:296:9: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/disasm/disassembly.py:316:10: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/disasm/disassembly.py:316:10: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/elf.py:358:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pwndbg/aglib/kernel/paging.py:326:34: warning[possibly-unbound-attribute] Attribute `stack` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/kernel/paging.py:326:34: warning[possibly-unbound-attribute] Attribute `stack` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/kernel/paging.py:346:30: warning[possibly-unbound-attribute] Attribute `TCR_EL1` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/kernel/paging.py:346:30: warning[possibly-unbound-attribute] Attribute `TCR_EL1` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/kernel/paging.py:392:34: warning[possibly-unbound-attribute] Attribute `vbar` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/kernel/paging.py:392:34: warning[possibly-unbound-attribute] Attribute `vbar` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/kernel/paging.py:578:34: warning[possibly-unbound-attribute] Attribute `stack` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/kernel/paging.py:578:34: warning[possibly-unbound-attribute] Attribute `stack` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/kernel/paging.py:603:25: warning[possibly-unbound-attribute] Attribute `TTBR1_EL1` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/kernel/paging.py:603:25: warning[possibly-unbound-attribute] Attribute `TTBR1_EL1` on type `Unknown | module | None` is possibly unbound
- pwndbg/aglib/kernel/paging.py:605:25: warning[possibly-unbound-attribute] Attribute `TTBR0_EL1` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/kernel/paging.py:605:25: warning[possibly-unbound-attribute] Attribute `TTBR0_EL1` on type `Unknown | module | None` is possibly unbound
+ pwndbg/aglib/vmmap_custom.py:137:54: error[invalid-argument-type] Argument to function `page_align` is incorrect: Expected `int`, found `Unknown | int | None`
- pwndbg/aglib/vmmap_custom.py:137:54: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/vmmap_custom.py:137:54: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
+ pwndbg/aglib/vmmap_custom.py:141:55: error[invalid-argument-type] Argument to function `page_align` is incorrect: Expected `int`, found `Unknown | int | None`
- pwndbg/aglib/vmmap_custom.py:141:55: warning[possibly-unbound-attribute] Attribute `sp` on type `Unknown | None` is possibly unbound
+ pwndbg/aglib/vmmap_custom.py:141:55: warning[possibly-unbound-attribute] Attribute `sp` on type `Unknown | module | None` is possibly unbound
- pwndbg/commands/dev.py:70:13: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/commands/dev.py:70:13: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/commands/hijack_fd.py:127:24: warning[possibly-unbound-attribute] Attribute `sp` on type `Unknown | None` is possibly unbound
+ pwndbg/commands/hijack_fd.py:127:24: warning[possibly-unbound-attribute] Attribute `sp` on type `Unknown | module | None` is possibly unbound
+ pwndbg/commands/hijack_fd.py:128:19: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int | None` and `int`
+ pwndbg/commands/hijack_fd.py:133:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int | None` and `Unknown | int | None`
- pwndbg/commands/hijack_fd.py:133:50: warning[possibly-unbound-attribute] Attribute `sp` on type `Unknown | None` is possibly unbound
+ pwndbg/commands/hijack_fd.py:133:50: warning[possibly-unbound-attribute] Attribute `sp` on type `Unknown | module | None` is possibly unbound
- pwndbg/commands/rop.py:27:77: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | None` is possibly unbound
+ pwndbg/commands/rop.py:27:77: warning[possibly-unbound-attribute] Attribute `pc` on type `Unknown | module | None` is possibly unbound
- pwndbg/commands/saved_register_frames.py:40:19: warning[possibly-unbound-attribute] Attribute `flags` on type `Unknown | None` is possibly unbound
+ pwndbg/commands/saved_register_frames.py:40:19: warning[possibly-unbound-attribute] Attribute `flags` on type `Unknown | module | None` is possibly unbound
- pwndbg/commands/saved_register_frames.py:41:25: warning[possibly-unbound-attribute] Attribute `flags` on type `Unknown | None` is possibly unbound
+ pwndbg/commands/saved_register_frames.py:41:25: warning[possibly-unbound-attribute] Attribute `flags` on type `Unknown | module | None` is possibly unbound
- pwndbg/dbg/gdb/__init__.py:156:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `Unknown | None`
+ pwndbg/dbg/gdb/__init__.py:156:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `Unknown | module | None`
- pwndbg/dbg/gdb/__init__.py:746:56: warning[possibly-unbound-attribute] Attribute `xpsr` on type `Unknown | None` is possibly unbound
+ pwndbg/dbg/gdb/__init__.py:746:56: warning[possibly-unbound-attribute] Attribute `xpsr` on type `Unknown | module | None` is possibly unbound
- pwndbg/dbg/lldb/__init__.py:146:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `Unknown | None`
+ pwndbg/dbg/lldb/__init__.py:146:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `Unknown | module | None`
- pwndbg/gdblib/functions.py:295:12: warning[possibly-unbound-attribute] Attribute `fsbase` on type `Unknown | None` is possibly unbound
+ pwndbg/gdblib/functions.py:295:12: warning[possibly-unbound-attribute] Attribute `fsbase` on type `Unknown | module | None` is possibly unbound
- pwndbg/gdblib/functions.py:327:12: warning[possibly-unbound-attribute] Attribute `gsbase` on type `Unknown | None` is possibly unbound
+ pwndbg/gdblib/functions.py:327:12: warning[possibly-unbound-attribute] Attribute `gsbase` on type `Unknown | module | None` is possibly unbound
- pwndbg/gdblib/ptmalloc2_tracking.py:702:9: warning[possibly-unbound-attribute] Attribute `delete` on type `Unknown | None` is possibly unbound
+ pwndbg/gdblib/ptmalloc2_tracking.py:702:9: warning[possibly-unbound-attribute] Attribute `delete` on type `Unknown | MallocEnterBreakpoint | None` is possibly unbound
- pwndbg/gdblib/ptmalloc2_tracking.py:703:9: warning[possibly-unbound-attribute] Attribute `delete` on type `Unknown | None` is possibly unbound
+ pwndbg/gdblib/ptmalloc2_tracking.py:703:9: warning[possibly-unbound-attribute] Attribute `delete` on type `Unknown | FreeEnterBreakpoint | None` is possibly unbound
- Found 2342 diagnostics
+ Found 2347 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/debug.py:324:5: error[invalid-assignment] Object of type `Unknown | None` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
+ asynq/debug.py:324:5: error[invalid-assignment] Object of type `Unknown | ((type[BaseException], BaseException, TracebackType | None, /) -> Any) | None` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
- asynq/tests/test_contexts.py:111:19: error[unresolved-reference] Name `expected_change_amount_base` used when not defined
- asynq/tests/test_contexts.py:113:23: error[unresolved-reference] Name `expected_change_amount_base` used when not defined
- asynq/tests/test_contexts.py:115:23: error[unresolved-reference] Name `expected_change_amount_base` used when not defined
- asynq/tests/test_contexts.py:118:27: error[unresolved-reference] Name `expected_change_amount_base` used when not defined
- asynq/tests/test_contexts.py:119:23: error[unresolved-reference] Name `expected_change_amount_base` used when not defined
- asynq/tests/test_contexts.py:120:19: error[unresolved-reference] Name `expected_change_amount_base` used when not defined
- Found 185 diagnostics
+ Found 179 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/Demos/ocx/ocxtest.py:144:21: error[unresolved-reference] Name `videoControlModule` used when not defined
- Pythonwin/pywin/Demos/ocx/ocxtest.py:159:36: error[unresolved-reference] Name `videoControlFileName` used when not defined
- Pythonwin/pywin/scintilla/view.py:281:9: warning[possibly-unbound-attribute] Attribute `configure` on type `Unknown | None` is possibly unbound
+ Pythonwin/pywin/scintilla/view.py:281:9: warning[possibly-unbound-attribute] Attribute `configure` on type `Unknown | ConfigManager | None` is possibly unbound
- Pythonwin/pywin/scintilla/view.py:282:12: warning[possibly-unbound-attribute] Attribute `last_error` on type `Unknown | None` is possibly unbound
+ Pythonwin/pywin/scintilla/view.py:282:12: warning[possibly-unbound-attribute] Attribute `last_error` on type `Unknown | ConfigManager | None` is possibly unbound
- Pythonwin/pywin/scintilla/view.py:283:32: warning[possibly-unbound-attribute] Attribute `last_error` on type `Unknown | None` is possibly unbound
+ Pythonwin/pywin/scintilla/view.py:283:32: warning[possibly-unbound-attribute] Attribute `last_error` on type `Unknown | ConfigManager | None` is possibly unbound
- Pythonwin/pywin/scintilla/view.py:342:23: warning[possibly-unbound-attribute] Attribute `get_key_binding` on type `Unknown | None` is possibly unbound
+ Pythonwin/pywin/scintilla/view.py:342:23: warning[possibly-unbound-attribute] Attribute `get_key_binding` on type `Unknown | ConfigManager | None` is possibly unbound
- Pythonwin/pywin/tools/browser.py:492:5: warning[possibly-unbound-attribute] Attribute `OpenObject` on type `Unknown | None` is possibly unbound
+ Pythonwin/pywin/tools/browser.py:492:5: warning[possibly-unbound-attribute] Attribute `OpenObject` on type `Unknown | BrowserTemplate | None` is possibly unbound
- com/win32comext/adsi/demos/search.py:38:8: warning[possibly-unbound-attribute] Attribute `verbose` on type `Unknown | None` is possibly unbound
+ com/win32comext/adsi/demos/search.py:38:8: warning[possibly-unbound-attribute] Attribute `verbose` on type `Unknown | Values | None` is possibly unbound
- com/win32comext/adsi/demos/search.py:44:16: warning[possibly-unbound-attribute] Attribute `user` on type `Unknown | None` is possibly unbound
+ com/win32comext/adsi/demos/search.py:44:16: warning[possibly-unbound-attribute] Attribute `user` on type `Unknown | Values | None` is possibly unbound
- com/win32comext/adsi/demos/search.py:44:30: warning[possibly-unbound-attribute] Attribute `password` on type `Unknown | None` is possibly unbound
+ com/win32comext/adsi/demos/search.py:44:30: warning[possibly-unbound-attribute] Attribute `password` on type `Unknown | Values | None` is possibly unbound
- com/win32comext/adsi/demos/search.py:76:8: warning[possibly-unbound-attribute] Attribute `attributes` on type `Unknown | None` is possibly unbound
+ com/win32comext/adsi/demos/search.py:76:8: warning[possibly-unbound-attribute] Attribute `attributes` on type `Unknown | Values | None` is possibly unbound
- com/win32comext/adsi/demos/search.py:77:22: warning[possibly-unbound-attribute] Attribute `attributes` on type `Unknown | None` is possibly unbound
+ com/win32comext/adsi/demos/search.py:77:22: warning[possibly-unbound-attribute] Attribute `attributes` on type `Unknown | Values | None` is possibly unbound
- com/win32comext/adsi/demos/search.py:81:26: warning[possibly-unbound-attribute] Attribute `filter` on type `Unknown | None` is possibly unbound
+ com/win32comext/adsi/demos/search.py:81:26: warning[possibly-unbound-attribute] Attribute `filter` on type `Unknown | Values | None` is possibly unbound
- com/win32comext/axdebug/adb.py:63:9: warning[possibly-unbound-attribute] Attribute `_OnSetBreakPoint` on type `Unknown | None` is possibly unbound
+ com/win32comext/axdebug/adb.py:63:9: warning[possibly-unbound-attribute] Attribute `_OnSetBreakPoint` on type `Unknown | Adb | None` is possibly unbound
- win32/Demos/win32netdemo.py:25:29: error[invalid-argument-type] Argument to function `NetUserDel` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:25:29: error[invalid-argument-type] Argument to function `NetUserDel` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:36:25: error[invalid-argument-type] Argument to function `NetUserAdd` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:36:25: error[invalid-argument-type] Argument to function `NetUserAdd` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:39:44: error[invalid-argument-type] Argument to function `NetUserChangePassword` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:39:44: error[invalid-argument-type] Argument to function `NetUserChangePassword` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:43:40: error[invalid-argument-type] Argument to function `NetUserChangePassword` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:43:40: error[invalid-argument-type] Argument to function `NetUserChangePassword` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:45:29: error[invalid-argument-type] Argument to function `NetUserDel` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:45:29: error[invalid-argument-type] Argument to function `NetUserDel` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:55:13: error[invalid-argument-type] Argument to function `NetUserEnum` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:55:13: error[invalid-argument-type] Argument to function `NetUserEnum` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:74:53: error[invalid-argument-type] Argument to function `NetGroupEnum` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:74:53: error[invalid-argument-type] Argument to function `NetGroupEnum` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:81:21: error[invalid-argument-type] Argument to function `NetGroupGetUsers` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:81:21: error[invalid-argument-type] Argument to function `NetGroupGetUsers` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:105:21: error[invalid-argument-type] Argument to function `NetLocalGroupGetMembers` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:105:21: error[invalid-argument-type] Argument to function `NetLocalGroupGetMembers` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:127:13: error[invalid-argument-type] Argument to function `NetServerEnum` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:127:13: error[invalid-argument-type] Argument to function `NetServerEnum` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:135:21: error[invalid-argument-type] Argument to function `NetShareEnum` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:135:21: error[invalid-argument-type] Argument to function `NetShareEnum` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:167:42: error[invalid-argument-type] Argument to function `NetLocalGroupAddMembers` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:167:42: error[invalid-argument-type] Argument to function `NetLocalGroupAddMembers` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:168:58: error[invalid-argument-type] Argument to function `NetLocalGroupGetMembers` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:168:58: error[invalid-argument-type] Argument to function `NetLocalGroupGetMembers` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:174:13: error[invalid-argument-type] Argument to function `NetLocalGroupDelMembers` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:174:13: error[invalid-argument-type] Argument to function `NetLocalGroupDelMembers` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:186:36: error[invalid-argument-type] Argument to function `NetUserGetInfo` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:186:36: error[invalid-argument-type] Argument to function `NetUserGetInfo` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:195:39: error[invalid-argument-type] Argument to function `NetUserGetInfo` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:195:39: error[invalid-argument-type] Argument to function `NetUserGetInfo` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:199:33: error[invalid-argument-type] Argument to function `NetUserSetInfo` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:199:33: error[invalid-argument-type] Argument to function `NetUserSetInfo` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:200:39: error[invalid-argument-type] Argument to function `NetUserGetInfo` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:200:39: error[invalid-argument-type] Argument to function `NetUserGetInfo` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Demos/win32netdemo.py:205:33: error[invalid-argument-type] Argument to function `NetUserSetInfo` is incorrect: Expected `str`, found `Unknown | None`
+ win32/Demos/win32netdemo.py:205:33: error[invalid-argument-type] Argument to function `NetUserSetInfo` is incorrect: Expected `str`, found `Unknown | str | None`
- win32/Lib/win32serviceutil.py:585:16: error[unresolved-reference] Name `g_debugService` used when not defined
- win32/Lib/win32serviceutil.py:587:9: error[unresolved-reference] Name `g_debugService` used when not defined
- Found 2004 diagnostics
+ Found 2000 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:1479:12: warning[possibly-unbound-attribute] Attribute `log` on type `Unknown | None` is possibly unbound
+ paasta_tools/utils.py:1479:12: warning[possibly-unbound-attribute] Attribute `log` on type `Unknown | LogWriter | None` is possibly unbound
- paasta_tools/utils.py:1502:12: warning[possibly-unbound-attribute] Attribute `log_audit` on type `Unknown | None` is possibly unbound
+ paasta_tools/utils.py:1502:12: warning[possibly-unbound-attribute] Attribute `log_audit` on type `Unknown | LogWriter | None` is possibly unbound

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:333:17: error[unresolved-reference] Name `asked` used when not defined
- lib/Crypto/SelfTest/Cipher/test_pkcs1_oaep.py:362:17: error[unresolved-reference] Name `mgfcalls` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:87:42: error[unresolved-reference] Name `Random` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:94:25: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:100:28: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:106:25: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:117:28: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:132:31: error[unresolved-reference] Name `size` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:138:13: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:139:18: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:151:31: error[unresolved-reference] Name `size` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:164:13: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:165:18: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:166:13: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:167:13: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:172:18: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:173:13: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:174:13: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:179:25: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:188:21: error[unresolved-reference] Name `DSA` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:191:21: error[unresolved-reference] Name `DSA` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:224:39: error[unresolved-reference] Name `DSA` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_DSA.py:234:39: error[unresolved-reference] Name `DSA` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:115:42: error[unresolved-reference] Name `Random` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:123:42: error[unresolved-reference] Name `Random` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:269:17: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:271:17: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:283:22: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:296:43: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:303:43: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:304:26: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:307:21: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/PublicKey/test_RSA.py:308:22: error[unresolved-reference] Name `bytes_to_long` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:38:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:39:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:43:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:46:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:47:39: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:50:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:53:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:56:13: error[unresolved-reference] Name `Counter` used when not defined
- lib/Crypto/SelfTest/Util/test_Counter.py:57:39: error[unresolved-reference] Name `Counter` used when not defined
- Found 1490 diagnostics
+ Found 1448 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/data/db.py:125:18: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~AlwaysTruthy & ~AlwaysFalsy) | (Client & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ openlibrary/data/db.py:127:40: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `Client`, in comparing `Unknown` with `(Unknown & ~AlwaysTruthy & ~AlwaysFalsy) | (Client & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ openlibrary/data/db.py:129:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `(Unknown & ~AlwaysTruthy & ~AlwaysFalsy) | (Client & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | dict[Unknown, Unknown]` is possibly unbound
- openlibrary/olbase/events.py:32:5: warning[possibly-unbound-attribute] Attribute `add_event_listener` on type `Unknown | None` is possibly unbound
+ openlibrary/olbase/events.py:32:5: warning[possibly-unbound-attribute] Attribute `add_event_listener` on type `Unknown | Infobase | None` is possibly unbound
- openlibrary/plugins/ol_infobase.py:44:9: warning[possibly-unbound-attribute] Attribute `add_event_listener` on type `Unknown | None` is possibly unbound
+ openlibrary/plugins/ol_infobase.py:44:9: warning[possibly-unbound-attribute] Attribute `add_event_listener` on type `Unknown | Infobase | None` is possibly unbound
- openlibrary/plugins/ol_infobase.py:46:5: warning[possibly-unbound-attribute] Attribute `add_event_listener` on type `Unknown | None` is possibly unbound
+ openlibrary/plugins/ol_infobase.py:46:5: warning[possibly-unbound-attribute] Attribute `add_event_listener` on type `Unknown | Infobase | None` is possibly unbound
- openlibrary/plugins/openlibrary/status.py:25:13: error[unresolved-reference] Name `feature_flags` used when not defined
- Found 698 diagnostics
+ Found 700 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/helper_tools/memoryleak.py:42:44: warning[possibly-unresolved-reference] Name `ssl` used when possibly not defined
- test/helper_tools/memoryleak.py:43:13: warning[possibly-unresolved-reference] Name `ssl` used when possibly not defined
- Found 1822 diagnostics
+ Found 1820 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- misc/python/materialize/scalability/executor/benchmark_executor.py:301:27: error[unresolved-reference] Name `next_worker_id` used when not defined
- misc/python/materialize/scalability/executor/benchmark_executor.py:302:26: error[unresolved-reference] Name `next_worker_id` used when not defined
- Found 3269 diagnostics
+ Found 3267 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/tests/test_ransac.py:228:16: error[unresolved-reference] Name `cause_skip` used when not defined
- Found 2021 diagnostics
+ Found 2020 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/vendor/ply/lex.py:1073:18: error[unresolved-reference] Name `token` used when not defined
- ddtrace/vendor/ply/yacc.py:368:21: error[unresolved-attribute] Type `<module 'ddtrace.vendor.ply.lex'>` has no attribute `lexer`
+ ddtrace/vendor/ply/yacc.py:368:21: warning[possibly-unbound-attribute] Attribute `lexer` on type `<module 'ddtrace.vendor.ply.lex'>` is possibly unbound
- ddtrace/vendor/ply/yacc.py:601:29: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | Unknown` is not assignable to attribute `lexer` on type `(Unknown & ~AlwaysFalsy & ~<Protocol with members 'lexer'>) | (YaccSymbol & ~AlwaysFalsy & ~<Protocol with members 'lexer'>)`
+ ddtrace/vendor/ply/yacc.py:601:29: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | Unknown | Lexer` is not assignable to attribute `lexer` on type `(Unknown & ~AlwaysFalsy & ~<Protocol with members 'lexer'>) | (YaccSymbol & ~AlwaysFalsy & ~<Protocol with members 'lexer'>)`
- ddtrace/vendor/ply/yacc.py:712:21: error[unresolved-attribute] Type `<module 'ddtrace.vendor.ply.lex'>` has no attribute `lexer`
+ ddtrace/vendor/ply/yacc.py:712:21: warning[possibly-unbound-attribute] Attribute `lexer` on type `<module 'ddtrace.vendor.ply.lex'>` is possibly unbound
- ddtrace/vendor/ply/yacc.py:907:29: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | Unknown` is not assignable to attribute `lexer` on type `(Unknown & ~AlwaysFalsy & ~<Protocol with members 'lexer'>) | (YaccSymbol & ~AlwaysFalsy & ~<Protocol with members 'lexer'>)`
+ ddtrace/vendor/ply/yacc.py:907:29: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | Unknown | Lexer` is not assignable to attribute `lexer` on type `(Unknown & ~AlwaysFalsy & ~<Protocol with members 'lexer'>) | (YaccSymbol & ~AlwaysFalsy & ~<Protocol with members 'lexer'>)`
- ddtrace/vendor/ply/yacc.py:1018:21: error[unresolved-attribute] Type `<module 'ddtrace.vendor.ply.lex'>` has no attribute `lexer`
+ ddtrace/vendor/ply/yacc.py:1018:21: warning[possibly-unbound-attribute] Attribute `lexer` on type `<module 'ddtrace.vendor.ply.lex'>` is possibly unbound
- ddtrace/vendor/ply/yacc.py:1199:29: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | Unknown` is not assignable to attribute `lexer` on type `(Unknown & ~AlwaysFalsy & ~<Protocol with members 'lexer'>) | (YaccSymbol & ~AlwaysFalsy & ~<Protocol with members 'lexer'>)`
+ ddtrace/vendor/ply/yacc.py:1199:29: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | Unknown | Lexer` is not assignable to attribute `lexer` on type `(Unknown & ~AlwaysFalsy & ~<Protocol with members 'lexer'>) | (YaccSymbol & ~AlwaysFalsy & ~<Protocol with members 'lexer'>)`
- tests/internal/test_forksafe.py:350:9: error[unresolved-reference] Name `service` used when not defined
- Found 6518 diagnostics
+ Found 6516 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/integrals/heurisch.py:285:17: warning[possibly-unbound-attribute] Attribute `has` on type `Unknown | None` is possibly unbound
+ sympy/integrals/heurisch.py:285:17: warning[possibly-unbound-attribute] Attribute `has` on type `Unknown | BesselTable | None` is possibly unbound
- sympy/integrals/heurisch.py:289:22: warning[possibly-unbound-attribute] Attribute `diffs` on type `Unknown | None` is possibly unbound
+ sympy/integrals/heurisch.py:289:22: warning[possibly-unbound-attribute] Attribute `diffs` on type `Unknown | BesselTable | None` is possibly unbound
- sympy/ntheory/partitions_.py:48:9: error[unresolved-reference] Name `_factor` used when not defined
- sympy/ntheory/partitions_.py:106:29: error[unresolved-reference] Name `_totient` used when not defined
- sympy/ntheory/partitions_.py:108:29: error[unresolved-reference] Name `_totient` used when not defined
- sympy/ntheory/partitions_.py:117:29: error[unresolved-reference] Name `_totient` used when not defined
- sympy/testing/runtests.py:2240:16: error[unresolved-reference] Name `linelen` used when not defined
- sympy/testing/runtests.py:2241:17: error[unresolved-reference] Name `text` used when not defined
- sympy/testing/runtests.py:2243:13: warning[possibly-unresolved-reference] Name `text` used when possibly not defined
- sympy/testing/runtests.py:2244:13: warning[possibly-unresolved-reference] Name `linelen` used when possibly not defined
- Found 12942 diagnostics
+ Found 12934 diagnostics

manticore (https://github.com/trailofbits/manticore)
- scripts/gdb.py:13:13: warning[possibly-unbound-attribute] Attribute `stdout` on type `Unknown | None` is possibly unbound
+ scripts/gdb.py:13:13: warning[possibly-unbound-attribute] Attribute `stdout` on type `Unknown | Popen[bytes] | None` is possibly unbound
+ scripts/gdb.py:13:13: warning[possibly-unbound-attribute] Attribute `read` on type `Unknown | IO[bytes] | None` is possibly unbound
+ scripts/gdb.py:14:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[""]` and `Unknown | bytes`
- scripts/gdb.py:39:5: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | None` is possibly unbound
+ scripts/gdb.py:39:5: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | Popen[bytes] | None` is possibly unbound
+ scripts/gdb.py:39:5: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | IO[bytes] | None` is possibly unbound
- scripts/gdb.py:40:5: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | None` is possibly unbound
+ scripts/gdb.py:40:5: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | Popen[bytes] | None` is possibly unbound
+ scripts/gdb.py:40:5: warning[possibly-unbound-attribute] Attribute `flush` on type `Unknown | IO[bytes] | None` is possibly unbound
- scripts/qemu.py:25:13: warning[possibly-unbound-attribute] Attribute `stdout` on type `Unknown | None` is possibly unbound
+ scripts/qemu.py:25:13: warning[possibly-unbound-attribute] Attribute `stdout` on type `Unknown | Popen[bytes] | None` is possibly unbound
+ scripts/qemu.py:25:13: warning[possibly-unbound-attribute] Attribute `read` on type `Unknown | IO[bytes] | None` is possibly unbound
+ scripts/qemu.py:26:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[""]` and `Unknown | bytes`
- scripts/qemu.py:91:9: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | None` is possibly unbound
+ scripts/qemu.py:91:9: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | Popen[bytes] | None` is possibly unbound
+ scripts/qemu.py:91:9: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | IO[bytes] | None` is possibly unbound
- scripts/qemu.py:92:5: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | None` is possibly unbound
+ scripts/qemu.py:92:5: warning[possibly-unbound-attribute] Attribute `stdin` on type `Unknown | Popen[bytes] | None` is possibly unbound
+ scripts/qemu.py:92:5: warning[possibly-unbound-attribute] Attribute `flush` on type `Unknown | IO[bytes] | None` is possibly unbound
- Found 1095 diagnostics
+ Found 1103 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/_lib/_ccallback.py:240:16: warning[possibly-unbound-attribute] Attribute `cast` on type `Unknown | None` is possibly unbound
+ scipy/_lib/_ccallback.py:240:16: warning[possibly-unbound-attribute] Attribute `cast` on type `Unknown | Literal[False] | None` is possibly unbound
- scipy/_lib/_ccallback.py:244:21: warning[possibly-unbound-attribute] Attribute `getctype` on type `Unknown | None` is possibly unbound
+ scipy/_lib/_ccallback.py:244:21: warning[possibly-unbound-attribute] Attribute `getctype` on type `Unknown | Literal[False] | None` is possibly unbound
- scipy/_lib/_ccallback.py:244:34: warning[possibly-unbound-attribute] Attribute `typeof` on type `Unknown | None` is possibly unbound
+ scipy/_lib/_ccallback.py:244:34: warning[possibly-unbound-attribute] Attribute `typeof` on type `Unknown | Literal[False] | None` is possibly unbound
- scipy/_lib/_ccallback.py:251:12: warning[possibly-unbound-attribute] Attribute `cast` on type `Unknown | None` is possibly unbound
+ scipy/_lib/_ccallback.py:251:12: warning[possibly-unbound-attribute] Attribute `cast` on type `Unknown | Literal[False] | None` is possibly unbound
- scipy/io/tests/test_mmio.py:46:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:47:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:48:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:52:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:53:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:54:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:69:42: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:70:42: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:170:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:171:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:172:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:176:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:177:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:178:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:196:42: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:197:42: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:275:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:276:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:277:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:281:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:283:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:372:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:373:55: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:375:42: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:377:17: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:572:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:573:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:634:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:636:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:639:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:655:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:663:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:679:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:687:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:697:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:699:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:702:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:713:9: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:715:22: error[unresolved-reference] Name `mminfo` used when not defined
- scipy/io/tests/test_mmio.py:718:13: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:741:17: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:742:26: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:755:17: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:781:5: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:796:5: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:799:9: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:805:5: error[unresolved-reference] Name `mmwrite` used when not defined
- scipy/io/tests/test_mmio.py:806:5: error[unresolved-reference] Name `mmread` used when not defined
- scipy/io/tests/test_mmio.py:829:38: error[unresolved-reference] Name `mmread` used when not defined
- Found 6398 diagnostics
+ Found 6349 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~424MB
+     memo fields = ~445MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-08-08 03:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jack%2Fsemantic-index-nested-bindings?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #19820 will **degrade performances by 7.39%**

<sub>Comparing <code>jack/semantic-index-nested-bindings</code> (abe6a36) with <code>main</code> (3a542a8)</sub>



### Summary

`⚡ 1` improvement  
`❌ 1` regression  
`✅ 40` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/jack%2Fsemantic-index-nested-bindings?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` ty_micro[many_string_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/jack%2Fsemantic-index-nested-bindings?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_string_assignments%3A%3Aty_micro%5Bmany_string_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 69.4 ms | 66.3 ms | +4.65% |
| ❌ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/jack%2Fsemantic-index-nested-bindings?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 828.7 ms | 894.8 ms | -7.39% |


---

_@oconnor663 reviewed on 2025-08-08 03:55_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 03:55_

If I delete this block, is there a good way to rerun the CodSpeed instrumentation and see if it helps?

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/builder.rs`:205 on 2025-08-08 03:57_

This tweak is necessary to avoid crashes in cases like this:

```py
def f():
    x = 1
    def g():
        nonlocal x
        x += 1
```

Previously we would exceed the cycle limit before we gave up on the int literal union and called `x` an `int`. This was @carljm's suggested fix.

---

_@oconnor663 reviewed on 2025-08-08 03:57_

---

_Comment by @github-actions[bot] on 2025-08-08 04:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ty` added by @MichaReiser on 2025-08-08 08:27_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:514 on 2025-08-08 08:36_

Too bad that `extract_if` requires Rust 1.88. But I think you can do this (almost, I broke some test. But something like this :)


```rust

// If we've popped a scope that free variables from nested (previously popped) scopes can
        // refer to (i.e. not a class body), try to resolve outstanding free variables.
        if kind.is_function_like() || popped_scope_id.is_global() {
            popped_free_variables.retain(|name, resolved_variables| {
                if let Some(symbol_id) = self.place_tables[popped_scope_id].symbol_id(name.as_str())
                {
                    // If a name is local or `global` here (i.e. bound or declared, and not marked
                    // `nonlocal`), then free variables of that name resolve here. Note that
                    // popping scopes in the normal stack order means that free variables resolve
                    // (correctly) to the closest scope with a matching definition.
                    let symbol = self.place_tables[popped_scope_id].symbol(symbol_id);

                    if symbol.is_local() || symbol.is_global() {
                        let is_global = symbol.is_global() || popped_scope_id.is_global();
                        let name = symbol.name().clone();

                        for FreeVariable {
                            scope_id: nested_scope_id,
                            nonlocal_identifier,
                        } in resolved_variables
                        {
                            if is_global {
                                if let Some(nonlocal_identifier) = nonlocal_identifier {
                                    // If the symbol is declared `nonlocal` in the nested scope (rather than
                                    // just used without a local binding or declaration), then it's a syntax
                                    // error for it to resolve to the global scope or to a `global` statement.
                                    self.report_semantic_error(SemanticSyntaxError {
                                        kind: SemanticSyntaxErrorKind::NoBindingForNonlocal(
                                            name.to_string(),
                                        ),
                                        range: nonlocal_identifier.range(),
                                        python_version: self.python_version,
                                    });

                                    continue;
                                }
                            }

                            let nested_place_table = &self.place_tables[*nested_scope_id];
                            let nested_symbol_id = nested_place_table.symbol_id(&name).unwrap();
                            let nested_symbol = nested_place_table.symbol(nested_symbol_id);
                            if nested_symbol.is_bound() {
                                self.place_tables[popped_scope_id]
                                    .add_nested_scope_with_binding(symbol_id, *nested_scope_id);
                            }
                        }

                        return false;
                    }
                }

                true
            });
        }
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:515 on 2025-08-08 08:36_

```suggestion
            let mut resolutions = Vec::new();
            let place_table = &self.place_tables[popped_scope_id];
            for name in popped_free_variables.keys() {
                if let Some(symbol_id) = place_table.symbol_id(name.as_str())
```

And replace it on line 521 too

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 08:59_

Not sure I understand. You can delete it and push the changes?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:585 on 2025-08-08 09:03_

Would it be sufficient to store the `non_local_identifier_range` in `FreeVariable`, given that we don't use the name (we always use the symbol name instead)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:606 on 2025-08-08 09:14_

It's a bit more fragile but I'd use `std::mem::take` here to work around the borrow checker issue rather than collecting the symbols

```rust
           let popped_place_table = std::mem::take(&mut self.place_tables[popped_scope_id]);

            for symbol in popped_place_table.symbols() {
                // Collect new implicit (not `nonlocal`) free variables.
                //
                // NOTE: Because these variables aren't bound (won't wind up in
                // `nested_scopes_with_bindings`) and aren't `nonlocal` (can't trigger `nonlocal`
                // syntax errors), collecting them currently has no effect. We could consider
                // removing this bit and renaming `free_variables` to say `unresolved_nonlocals`?
                if symbol.is_used()
                    && !symbol.is_bound()
                    && !symbol.is_declared()
                    && !symbol.is_global()
                    // `nonlocal` variables are handled in `visit_stmt`, which lets us stash an AST
                    // reference.
                    && !symbol.is_nonlocal()
                {
                    parent_free_variables
                        .entry(symbol.name().clone())
                        .or_default()
                        .push(FreeVariable {
                            scope_id: popped_scope_id,
                            nonlocal_identifier: None,
                        });
                }

                // Record bindings of global variables. Put these in a temporary Vec as another
                // borrowck workaround.
                if symbol.is_global() && symbol.is_bound() {
                    // Add this symbol to the global scope, if it isn't there already.
                    let global_symbol_id =
                        self.add_symbol_to_scope(symbol.name().clone(), FileScopeId::global());
                    // Update the global place table with this reference. Doing this here rather than
                    // when we first encounter the `global` statement lets us see whether the symbol is
                    // bound.
                    self.place_tables[FileScopeId::global()]
                        .add_nested_scope_with_binding(global_symbol_id, popped_scope_id);
                }
            }

            self.place_tables[popped_scope_id] = popped_place_table;

```

You'll need to apply the same trick to `parent_free_variables` or iterate over symbols twice

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 09:16_

I don't fully understand the implications of this, but I've a very strong preference not to store anything in the semantic index for which we currently have no use for. 

The semantic index is already very complicated and it's not very obvious which information is used for what. That makes it non-trivial to identify unused data that could be removed. That's why I rather not add data until it's absolutely necessary because removing it later is hard (it's not like Rust tells us that it's unused)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:165 on 2025-08-08 09:21_

How common are these? Could we use a `ThinVec` instead? 

Given that we collect all entries in the builder. Could we store them in two vecs where:

1. The first vec is a list of `ScopedSymbolId's with a range into the second vec
2. The second vec stores the `FileScopeId`. All elements for the same `ScopeSymbolId` are added next to each other.

This reduces the cost two exactly two (thin vecs?) rather than a HashMap (which is larger than a vec) with nested vecs

An alternative is to use a `SmallVec` for the inner `Vec`, given that `FileScopeId` is a u32 (meaning up to 4 elements fit into an inline vec)


It's hard for me to advise on the best representation without knowing more about how many elements we expect (per scope and per symbol)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:278 on 2025-08-08 09:21_

We should also shrink all inner vecs

---

_@MichaReiser reviewed on 2025-08-08 09:24_

ty. 

I only reviewed the code changes. I'd like to have a better understanding of the memory regression before merging this. Is it because we now have a better understanding of the code or is it because the semantic index grows that much. If it's the latter, than I think that's more than I would expect from this change. 

Would you mind collecting and comparing the full memory report on a large repository and sharing it here.

---

_@oconnor663 reviewed on 2025-08-08 13:51_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 13:51_

I've opened a dummy PR for this: https://github.com/astral-sh/ruff/pull/19828

---

_@oconnor663 reviewed on 2025-08-08 14:17_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 14:17_

These references don't get stored in the index itself, only in the builder. Does that make it better? :)

---

_@MichaReiser reviewed on 2025-08-08 14:21_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 14:21_

Not necessarily. Memory access isn't cheap compared to pure computations

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:514 on 2025-08-08 14:22_

I feel like I tried this a few different ways and couldn't get it to work. But you're right, it works with `retain`. Maybe I was too nervous about cloning symbols or looking them up more than once? Can't remember.

---

_@oconnor663 reviewed on 2025-08-08 14:22_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:606 on 2025-08-08 15:28_

This is a temporary `Vec` that's empty (doesn't allocate) in scopes that don't bind a `global` variable. I think the performance and memory cost of it should be negligible. Are you sure it's worth these sorts of hijinks to avoid it? :)

---

_@oconnor663 reviewed on 2025-08-08 15:28_

---

_@MichaReiser reviewed on 2025-08-08 15:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:606 on 2025-08-08 15:32_

I don't know. Codspeed might tell you 😆 

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:165 on 2025-08-08 16:03_

The keys of this map are the IDs of symbols in this scope that have one or more bindings in nested scopes (via `nonlocal` or `global` references in those nested scopes). So for the (vast?) majority of scopes / symbol tables, this map will be empty. Most variables that do have nonlocal bindings probably only have one or two, so a `SmallVec` might be a good optimization for the inner `Vec`, but if the map is empty most of the time it also might not matter?

If we're worried about the footprint of the map in the `SymbolTable` struct (that is, even when it's empty), we could consider a couple other options:

- Wrap the map inside an `Option<Box<...>>`. That keeps the space overhead down to one pointer in `SymbolTable`, at the cost of an extra allocation when we do populate the table.
- Get rid of the per-symbol-table maps and put a file-wide map at the top level of the `SemanticIndex` with keys like `(FileScopeId, ScopedSymbolId)`. Then to cut down on unnecessary lookups in that table, maybe allocate a bit in `SymbolFlags` called something like `HAS_BINDINGS_IN_NESTED_SCOPES`?

But stepping back from all that, is it possible that [the memory usage stats I just posted below](https://github.com/astral-sh/ruff/pull/19820#issuecomment-3168443938) tell us we don't need to worry about this? What else should I measure?

---

_@oconnor663 reviewed on 2025-08-08 16:03_

---

_Comment by @oconnor663 on 2025-08-08 16:08_

> Would you mind collecting and comparing the full memory report on a large repository and sharing it here.

Here's the summary of running `ty check app` in my local copy of a large project, first on `main`:

```
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 18136.92MB
    struct metadata = 856.62MB
    struct fields = 971.76MB
    memo metadata = 3787.71MB
    memo fields = 12520.82MB
```

And with this PR:

```
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 18196.01MB
    struct metadata = 857.17MB
    struct fields = 972.24MB
    memo metadata = 3798.75MB
    memo fields = 12567.84MB
```

It seems like the change is small? But I'm new to these stats, and I'm not sure I'm measuring the right thing.

---

_Comment by @MichaReiser on 2025-08-08 16:09_

That's good :). Can you run the full report. I'm curious which specific structs are increasing. Can you run it on prefect which shows up in the mypy primer check result

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:81 on 2025-08-08 16:11_

This seems not-ideal, but it may not be easy to get rid of, and I guess given it results from a type error, maybe it's fine -- fix the type error and it will go away. (I assume this is coming from the fact that we infer `Unknown` from the invalid assignment below.)

---

_Comment by @oconnor663 on 2025-08-08 16:17_

Here are full reports from checking https://github.com/PrefectHQ/prefect. On our `main`:

<details>

```
Found 6192 diagnostics
=======SALSA STRUCTS=======
`Definition`                                       metadata=7.42MB   fields=14.83MB  count=185421
`Expression`                                       metadata=5.99MB   fields=6.98MB   count=124701
`IntersectionType`                                 metadata=1.10MB   fields=2.20MB   count=19636
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=1.46MB   fields=1.25MB   count=26111
`OverloadLiteral`                                  metadata=1.23MB   fields=1.05MB   count=21963
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=1.14MB   fields=0.98MB   count=20401
`FunctionType`                                     metadata=1.59MB   fields=0.68MB   count=28385
`File`                                             metadata=0.52MB   fields=0.52MB   count=8164
`place_by_id::interned_arguments`                  metadata=1.26MB   fields=0.48MB   count=24145
`ScopeId`                                          metadata=1.08MB   fields=0.46MB   count=38673
`StringLiteralType`                                metadata=1.56MB   fields=0.45MB   count=27901
`Type < 'db >::apply_specialization_::interned_arguments` metadata=0.91MB   fields=0.39MB   count=16289
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=0.45MB   fields=0.38MB   count=7993
`FunctionLiteral`                                  metadata=1.23MB   fields=0.35MB   count=22029
`UnionType`                                        metadata=1.09MB   fields=0.31MB   count=19388
`TupleType`                                        metadata=0.25MB   fields=0.29MB   count=4542
`ClassLiteral`                                     metadata=0.28MB   fields=0.24MB   count=5026
`CallableType`                                     metadata=0.14MB   fields=0.23MB   count=2580
`Specialization`                                   metadata=0.36MB   fields=0.21MB   count=6492
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=0.48MB   fields=0.14MB   count=8644
`TypeVarInstance`                                  metadata=0.09MB   fields=0.13MB   count=1617
`BoundMethodType`                                  metadata=0.25MB   fields=0.11MB   count=4484
`GenericAlias`                                     metadata=0.31MB   fields=0.09MB   count=5550
`ModuleLiteralType`                                metadata=0.18MB   fields=0.07MB   count=3376
`Unpack`                                           metadata=0.06MB   fields=0.07MB   count=1391
`ModuleNameIngredient`                             metadata=0.08MB   fields=0.05MB   count=1412
`GenericContext`                                   metadata=0.04MB   fields=0.04MB   count=767
`FileModule`                                       metadata=0.02MB   fields=0.04MB   count=761
`StarImportPlaceholderPredicate`                   metadata=0.03MB   fields=0.02MB   count=1135
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=0.07MB   fields=0.02MB   count=1221
`PropertyInstanceType`                             metadata=0.02MB   fields=0.01MB   count=355
`EnumLiteralType`                                  metadata=0.01MB   fields=0.01MB   count=192
`BoundSuperType`                                   metadata=0.01MB   fields=0.00MB   count=169
`ProtocolInterface`                                metadata=0.01MB   fields=0.00MB   count=180
`TypeIsType`                                       metadata=0.01MB   fields=0.00MB   count=116
`BytesLiteralType`                                 metadata=0.00MB   fields=0.00MB   count=76
`TypedDictType`                                    metadata=0.00MB   fields=0.00MB   count=29
`Program`                                          metadata=0.00MB   fields=0.00MB   count=1
`FieldInstance`                                    metadata=0.00MB   fields=0.00MB   count=6
`Project`                                          metadata=0.00MB   fields=0.00MB   count=1
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=1
`DeprecatedInstance`                               metadata=0.00MB   fields=0.00MB   count=2
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=0
`PatternPredicate`                                 metadata=0.00MB   fields=0.00MB   count=0
`PEP695TypeAliasType`                              metadata=0.00MB   fields=0.00MB   count=0
`BareTypeAliasType`                                metadata=0.00MB   fields=0.00MB   count=0
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
`dynamic_resolution_paths::interned_arguments`     metadata=0.00MB   fields=0.00MB   count=1
`merge_overrides::interned_arguments`              metadata=0.00MB   fields=0.00MB   count=0
=======SALSA QUERIES=======
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=24.16MB  fields=10.45MB  count=145081
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=35.81MB  fields=4.51MB   count=112657
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=12.59MB  fields=1.16MB   count=29115
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=1.99MB   fields=0.92MB   count=11465
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=4.26MB   fields=0.84MB   count=26111
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=1.64MB   fields=0.77MB   count=24145
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=4.27MB   fields=0.65MB   count=20401
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=2.60MB   fields=0.65MB   count=20185
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=12.79MB  fields=0.56MB   count=1665
`infer_expression_type -> ty_python_semantic::types::Type`
    metadata=2.44MB   fields=0.51MB   count=32138
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=1.16MB   fields=0.43MB   count=18068
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=1.93MB   fields=0.42MB   count=5799
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=1.64MB   fields=0.35MB   count=8644
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=0.92MB   fields=0.26MB   count=16289
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=0.97MB   fields=0.23MB   count=4895
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.10MB   fields=0.19MB   count=1500
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=4.35MB   fields=0.19MB   count=7993
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.70MB   fields=0.18MB   count=5730
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=0.70MB   fields=0.17MB   count=1961
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=0.26MB   fields=0.14MB   count=1390
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.37MB   fields=0.08MB   count=5013
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.52MB   fields=0.08MB   count=9930
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=0.36MB   fields=0.04MB   count=5013
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap>`
    metadata=0.26MB   fields=0.04MB   count=5000
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=0.33MB   fields=0.04MB   count=1221
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.11MB   fields=0.03MB   count=1735
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=0.13MB   fields=0.03MB   count=1669
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.53MB   fields=0.02MB   count=1500
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.41MB   fields=0.02MB   count=1412
`source_text -> ruff_db::source::SourceText`
    metadata=0.10MB   fields=0.01MB   count=1669
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=0.08MB   fields=0.01MB   count=1522
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.11MB   fields=0.01MB   count=1500
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.06MB   fields=0.01MB   count=1062
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.47MB   fields=0.00MB   count=4968
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.35MB   fields=0.00MB   count=4759
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.01MB   fields=0.00MB   count=121
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=35
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.00MB   count=39
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.01MB   fields=0.00MB   count=50
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
```

</details>

```
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 595.05MB
    struct metadata = 30.75MB
    struct fields = 33.10MB
    memo metadata = 119.49MB
    memo fields = 411.71MB
```

With this PR:

<details>

```
Found 6194 diagnostics
=======SALSA STRUCTS=======
`Definition`                                       metadata=7.42MB   fields=14.83MB  count=185421
`Expression`                                       metadata=5.99MB   fields=6.98MB   count=124701
`IntersectionType`                                 metadata=1.12MB   fields=2.23MB   count=19921
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=1.48MB   fields=1.27MB   count=26358
`OverloadLiteral`                                  metadata=1.23MB   fields=1.05MB   count=21963
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=1.16MB   fields=0.99MB   count=20646
`FunctionType`                                     metadata=1.59MB   fields=0.68MB   count=28383
`File`                                             metadata=0.52MB   fields=0.52MB   count=8164
`place_by_id::interned_arguments`                  metadata=1.26MB   fields=0.49MB   count=24306
`ScopeId`                                          metadata=1.08MB   fields=0.46MB   count=38673
`StringLiteralType`                                metadata=1.56MB   fields=0.45MB   count=27901
`Type < 'db >::apply_specialization_::interned_arguments` metadata=0.91MB   fields=0.39MB   count=16290
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=0.45MB   fields=0.38MB   count=7993
`FunctionLiteral`                                  metadata=1.23MB   fields=0.35MB   count=22029
`UnionType`                                        metadata=1.15MB   fields=0.33MB   count=20457
`TupleType`                                        metadata=0.25MB   fields=0.29MB   count=4541
`ClassLiteral`                                     metadata=0.28MB   fields=0.24MB   count=5026
`CallableType`                                     metadata=0.14MB   fields=0.23MB   count=2578
`Specialization`                                   metadata=0.36MB   fields=0.21MB   count=6490
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=0.48MB   fields=0.14MB   count=8644
`TypeVarInstance`                                  metadata=0.09MB   fields=0.13MB   count=1615
`BoundMethodType`                                  metadata=0.25MB   fields=0.11MB   count=4484
`GenericAlias`                                     metadata=0.31MB   fields=0.09MB   count=5549
`ModuleLiteralType`                                metadata=0.18MB   fields=0.07MB   count=3376
`Unpack`                                           metadata=0.06MB   fields=0.07MB   count=1391
`ModuleNameIngredient`                             metadata=0.08MB   fields=0.05MB   count=1412
`GenericContext`                                   metadata=0.04MB   fields=0.04MB   count=766
`FileModule`                                       metadata=0.02MB   fields=0.04MB   count=761
`StarImportPlaceholderPredicate`                   metadata=0.03MB   fields=0.02MB   count=1135
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=0.07MB   fields=0.02MB   count=1221
`PropertyInstanceType`                             metadata=0.02MB   fields=0.01MB   count=355
`EnumLiteralType`                                  metadata=0.01MB   fields=0.01MB   count=192
`BoundSuperType`                                   metadata=0.01MB   fields=0.00MB   count=169
`ProtocolInterface`                                metadata=0.01MB   fields=0.00MB   count=180
`TypeIsType`                                       metadata=0.01MB   fields=0.00MB   count=116
`BytesLiteralType`                                 metadata=0.00MB   fields=0.00MB   count=76
`TypedDictType`                                    metadata=0.00MB   fields=0.00MB   count=29
`Program`                                          metadata=0.00MB   fields=0.00MB   count=1
`FieldInstance`                                    metadata=0.00MB   fields=0.00MB   count=6
`Project`                                          metadata=0.00MB   fields=0.00MB   count=1
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=1
`DeprecatedInstance`                               metadata=0.00MB   fields=0.00MB   count=2
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=0
`PatternPredicate`                                 metadata=0.00MB   fields=0.00MB   count=0
`PEP695TypeAliasType`                              metadata=0.00MB   fields=0.00MB   count=0
`BareTypeAliasType`                                metadata=0.00MB   fields=0.00MB   count=0
`dynamic_resolution_paths::interned_arguments`     metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
`merge_overrides::interned_arguments`              metadata=0.00MB   fields=0.00MB   count=0
=======SALSA QUERIES=======
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=24.78MB  fields=10.45MB  count=145081
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=35.83MB  fields=4.51MB   count=112657
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=12.59MB  fields=1.16MB   count=29115
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=1.99MB   fields=0.92MB   count=11466
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=4.29MB   fields=0.84MB   count=26358
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=2.27MB   fields=0.78MB   count=24306
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=4.34MB   fields=0.66MB   count=20646
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=2.61MB   fields=0.65MB   count=20192
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=12.79MB  fields=0.56MB   count=1665
`infer_expression_type -> ty_python_semantic::types::Type`
    metadata=2.44MB   fields=0.51MB   count=32150
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=1.16MB   fields=0.43MB   count=18068
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=1.93MB   fields=0.42MB   count=5800
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=1.64MB   fields=0.35MB   count=8644
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=0.92MB   fields=0.26MB   count=16290
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=0.97MB   fields=0.23MB   count=4895
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.10MB   fields=0.19MB   count=1500
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=4.35MB   fields=0.19MB   count=7993
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.70MB   fields=0.18MB   count=5760
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=0.70MB   fields=0.17MB   count=1961
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=0.26MB   fields=0.14MB   count=1390
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.52MB   fields=0.08MB   count=10051
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.37MB   fields=0.08MB   count=5013
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap>`
    metadata=0.27MB   fields=0.04MB   count=5144
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=0.36MB   fields=0.04MB   count=5013
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=0.33MB   fields=0.04MB   count=1221
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.11MB   fields=0.03MB   count=1735
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=0.13MB   fields=0.03MB   count=1669
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.53MB   fields=0.02MB   count=1500
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.41MB   fields=0.02MB   count=1412
`source_text -> ruff_db::source::SourceText`
    metadata=0.10MB   fields=0.01MB   count=1669
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=0.08MB   fields=0.01MB   count=1522
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.11MB   fields=0.01MB   count=1500
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.06MB   fields=0.01MB   count=1062
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.47MB   fields=0.00MB   count=4968
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.35MB   fields=0.00MB   count=4759
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.01MB   fields=0.00MB   count=121
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.00MB   count=35
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.00MB   count=39
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.01MB   fields=0.00MB   count=50
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
```

</details>

```
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 603.66MB
    struct metadata = 30.86MB
    struct fields = 33.18MB
    memo metadata = 120.90MB
    memo fields = 418.72MB
```

---

_Comment by @MichaReiser on 2025-08-08 16:19_

Diff: https://www.diffchecker.com/6QvuZnFW/

---

_Comment by @MichaReiser on 2025-08-08 16:21_

Looking at the diff, it seems mainly to be due to changes in type inference but not to the size increase in semantic index itself. Consider my concerns resolved. Thanks for digging into this

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:165 on 2025-08-08 16:22_

I think we expect this map to be empty for most scopes, and when it is not empty, to have only one or two keys in it, and each key's value to likely also have only one or two entries in it. So overall, we expect this to be small. I think this means both that we could probably find a more efficient representation here, and that it may not matter much to overall memory usage in practice (as suggested by the memory usage stats). I'll defer to @MichaReiser on whether it's worth pursuing an alternative representation here. My own feeling is that the memory increase here seems minimal, and I'm more concerned about reducing the performance regression (for which I think the focus should likely be on `place_by_id`.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:205 on 2025-08-08 16:27_

I think it's not ideal that we will do 100 fixpoint iterations in a case like this before falling back, but I also think improving that is out of scope for this PR.

I think the best way to improve this is actually not cycle-specific; I think we should place a much lower limit on the size of literal integer unions we are willing to explode and perform literal math on, and above that limit we just fallback to int -- and similar for operations on other literal types for which we make precise inferences. The rationale here is that we'd like to have a reasonably large limit on the size of literal unions that users can explicitly create, but it's reasonable to have a much lower limit on the size of literal unions that we implicitly create via precise type inference of operations.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:657 on 2025-08-08 16:37_

In other places (e.g. `place_from_bindings_impl`) we currently go to a bit of trouble to avoid allocating a `UnionBuilder` in the most common case (one visible binding) where it won't be needed. We have seen in past PRs that this has noticeable performance impact, because this path is so hot.

I suspect that the changes in this function, rather than anything in the semantic index building, are the most likely cause of the benchmark slowdowns in this PR. I think this is supported by the CodSpeed execution profile on anyio, where the regressing functions all seem to be in type inference, not semantic index building.

I think it would be worth some effort here to optimize the case where there are no nested bindings to consider. Ideally, in the no-nested-bindings scenario, this function would do exactly one hash-table lookup more than it did before this PR. (Or possibly even less than that, e.g. maybe a scope-level "this scope has nested writes" flag that allows us to bypass all additional work if it's not set?)

We should also be able to do zero additional work here in the case where the name has a declared type in the outer scope. In that scenario we should just use the declared type and not query the nonlocal bindings at all. Currently this PR doesn't _use_ the types of the nonlocal bindings in that case, but it queries them anyway. That's wasteful, and I suspect cleaning it up will address much of the regression in anyio (which appears to have [exactly one `nonlocal` declaration](https://github.com/agronholm/anyio/blob/fef093c1e33563493182b95a0c7028b86c61dd90/src/anyio/_core/_sockets.py#L180), but of a name which has a declared type in its enclosing scope, so shouldn't require us to do any additional work.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:797 on 2025-08-08 16:39_

I wonder here why we aren't just adding the `inferred` type (if any) to `nested_bindings_union`, before building it?

But don't pay too much attention to this comment, since I suspect this will all change anyway if we try to optimize the happy path in this function better.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:614 on 2025-08-08 17:03_

I would be inclined to remove the unnecessary collection and do the rename, for now. It seems easy to reverse later if/when we have a use for the fuller data, and it just seems like a good principle (both for readability/maintainability and performance) to avoid doing unnecessary work, if it's easy not to do it.

---

_@carljm requested changes on 2025-08-08 17:09_

Thank you! I love the simplifications in type inference, and the fact that we now match mypy and pyright's inference capability in cases with nonlocal writes. The collection logic in the semantic index seems clear to me, though I do think we should streamline it to avoid doing any extra work for things we don't use.

I think we do need to better understand/fix the anyio regression, and it looks like we have a 1% regression overall in the cold-check benchmark, which I believe checks a codebase that doesn't use `nonlocal`. I don't think we should pay any detectable performance penalty for this PR in code that doesn't use `nonlocal`, and I think the main focus to achieve that needs to be in better optimizing the no-nonlocal-writes path in `place_by_id`.

---

_@carljm reviewed on 2025-08-08 17:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:205 on 2025-08-08 17:19_

(I filed https://github.com/astral-sh/ty/issues/957 to track this separately.)

---

_Comment by @oconnor663 on 2025-08-08 22:05_

@carljm I'm able to reduce the 10% slowdown with https://github.com/agronholm/anyio locally (though I have to set `export RAYON_NUM_THREADS=1` to see it). When I play with making my use of `UnionBuilder` lazier, or optimizing `UnionBuilder` by `SmallVec` more internally, none of that seems to have much effect. But when I shrink `MAX_UNION_LITERALS` down to a tiny value like 10, that gets rid of almost all the slowdown without any other changes. Other than that, I haven't been able to figure out exactly where the slowdown is happening, other than that there seems to be a decent number of `nonlocal` variables in this repo. Any chance it's obvious to you?

---

_Comment by @carljm on 2025-08-08 22:34_

@oconnor663 Ah, hm, I thought the repo only had one `nonlocal` because I mistakenly searched only in the source code, not in the tests. Looks like there are tons of nonlocal in the tests. But it still looks like a lot of those have declared types in the enclosing scope, which should (but doesn't currently) mean that we don't do any extra work for them in this PR.

> When I play with making my use of `UnionBuilder` lazier, or optimizing `UnionBuilder` by `SmallVec` more internally

What about ensuring that we never query nonlocal bindings at all if the symbol has declarations in the outer scope and we aren't going to use the nonlocal bindings anyway? That seems like the highest-priority perf improvement, which we should do regardless. Have you implemented that yet? How much does that impact perf, if at all?

> when I shrink `MAX_UNION_LITERALS` down to a tiny value like 10, that gets rid of almost all the slowdown without any other changes

Interesting! That strongly suggests that we are hitting a cycle in the nonlocal bindings where we grow a literal union on every iteration of the cycle. I did some grep-based spot-checking of the `nonlocal` uses and didn't come across any that jumped out at me, but I didn't check every `nonlocal`. I would expect to find something like a `nonlocal_var += ...` where `...` is a literal integer or literal string or literal bytestring.

If this is really the only thing that fixes it, that suggests that we would need https://github.com/astral-sh/ty/issues/957 to properly reclaim this perf. But I would love to actually identify the code that is actually causing this, to make sure we aren't missing something. One way to do this could be to benchmark each test file individually, with main vs this PR, to narrow down the file in which the problem occurs. (I think this can be done non-painfully by pre-building release versions of main and this PR, and then using `hyperfine` so each test is just a single CLI command; could even script looping over all the files that contain `nonlocal` and running this benchmark command against them.) And if it's still not clear, can start removing chunks of that file and see when the discrepancy goes away.

> though I have to set `export RAYON_NUM_THREADS=1` to see it

Yes, that's generally the right thing to do for reliable benchmarking (if your goal is to improve straight-line perf, not specifically to improve parallelism).

---

_Comment by @oconnor663 on 2025-08-08 22:54_

> and it looks like we have a 1% regression overall in the cold-check benchmark

I see that in the CodSpeed report, but I wonder if it's within the error bars. When I try to reproduce it locally (`cargo bench --bench ty cold` correct?), I have to close my browser and disable CPU frequency boost to get consistent results within a ~tenth of a percent, and when I do that I don't see any consistent difference between my branch and main on that specific benchmark.

> What about ensuring that we never query nonlocal bindings at all if the symbol has declarations in the outer scope and we aren't going to use the nonlocal bindings anyway?

I thought I had tested this above and found it didn't help, but I tried it again just now and it does. Locally it looks like it recovers about half of the slowdown, but definitely not all of it. I'll push the change and see what happens in CodSpeed.

> But I would love to actually identify the code that is actually causing this, to make sure we aren't missing something.

Yeah I'd like to know too. I'll see if I can divide and conquer it somehow.

---

_Comment by @carljm on 2025-08-08 23:14_

> I see that in the CodSpeed report, but I wonder if it's within the error bars. When I try to reproduce it locally (`cargo bench --bench ty cold` correct?), I have to close my browser and disable CPU frequency boost to get consistent results within a ~tenth of a percent, and when I do that I don't see any consistent difference between my branch and main on that specific benchmark.

This is an instrumented benchmark on CodSpeed, not a walltime benchmark. That means it's counting CPU instructions (and I think assigning some fixed cost estimate to syscalls?). In some ways that makes it less "real-world performance", but it also generally makes it pretty consistent; I don't usually see a lot of fluctuation in those. But if it really doesn't show up locally at all, maybe that's a case where CodSpeed is misleading us a bit.

---

_Comment by @oconnor663 on 2025-08-08 23:19_

Ah very neat. In that case I'm not sure who's right :sweat_smile:

The re-run speed report shows the Anyio regression at 7% now instead of 10%, so laziness did help a bit. Still digging into the specifics.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:165 on 2025-08-10 12:50_

I don't feel strongly about it but a `ThinVec` could be a great use case because it reduces the overhead per scope from 48 bytes to 8 (if empty) and a linear scan should be cheap. 

Constructing such a vec based map from a real map (it's fine to use a proper map during building) should be trivial. It's only the lookup that requires a scan (but you could even consider to use a `ThinVec` during building, which might be cheaper, but I don't care much bout that)

---

_@MichaReiser reviewed on 2025-08-10 12:50_

---

_@MichaReiser reviewed on 2025-08-10 12:51_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:205 on 2025-08-10 12:51_

Would it make sense to make this change in a separate PR to understand its performance and ecosystem impact?

---

_Comment by @MichaReiser on 2025-08-10 12:58_

One thing you could try is to record a profile locally using simply (with `samply record cargo bench ...`) and:

* Invert the call graph, see if both versions have the same key performance driver
* Use the compare feature of firefox profiler to compare two recorded profiles

I often find this more useful than codspeed's profile because codspeed filters and truncates the stack frames which sometimes is great but sometimes hides exactly what you're supposed to see


---

_Comment by @AlexWaygood on 2025-08-10 13:00_

We have some notes in an internal Notion document here with some tips 'n' tricks for profiling and benchmarking ty locally: https://www.notion.so/astral-sh/Performance-measurements-15848797e1ca808f97c9d928e6923d03?source=copy_link

---

_Comment by @oconnor663 on 2025-08-11 15:09_

Ok some progress, deleting lots of code and re-benching narrowed the entire (remaining) slowdown to this generator-context-manager: https://github.com/agronholm/anyio/blob/fef093c1e33563493182b95a0c7028b86c61dd90/src/anyio/pytest_plugin.py#L37-L64

---

_Comment by @oconnor663 on 2025-08-11 16:45_

> I would expect to find something like a `nonlocal_var += ...` where `...` is a literal integer or literal string or literal bytestring.

[Found it!](https://github.com/agronholm/anyio/blob/fef093c1e33563493182b95a0c7028b86c61dd90/src/anyio/pytest_plugin.py#L56-L60) `_runner_leases` is an un-annotated `global` integer, and there's a `+= 1` on it (`-= 1` too). I think this means we understand why the performance regression is happening, and we plan on fixing it separately from this PR?

---

_Comment by @carljm on 2025-08-11 16:59_

> I think this means we understand why the performance regression is happening, and we plan on fixing it separately from this PR?

Yes! That means the bulk of the anyio regression is https://github.com/astral-sh/ty/issues/957. It might mean that if we land this for beta, we will also need to fix that for beta, depending on how this shows up in user codebases.

I do feel there's still room to improve the performance here outside of that particular anyio issue. Currently this is showing a [very consistent 1-2% across-the-board regression in instrumented CodSpeed benchmarks](https://codspeed.io/astral-sh/ruff/branches/jack%2Fsemantic-index-nested-bindings?runnerMode=Instrumentation). Apparently local walltime runs aren't sensitive enough to show this, but I feel like there are still some clear things we could do to improve here. We are no longer taking on the overhead of allocating a UnionBuilder or querying nonlocal types if we won't use the nonlocal types, which is good. We are still (within the lazy closure) eagerly allocating a UnionBuilder before we even know whether there are any nonlocal bindings to consider, in any case where we would consider nonlocal bindings.

I think it should be possible for this PR to show effectively zero performance impact on a codebase that doesn't use any`global` or `nonlocal` (which includes, for example, `tomllib`, the codebase we run our basic `ty_check[cold]` benchmark on), and I think that's the target we should aim for before landing this.

I'm fine with setting this aside for the moment and moving on to NewType. I do consider this a worthwhile feature (it brings us to parity with mypy/pyright), but it's probably not critical for the beta. If we put it on ice for now, hopefully it doesn't accumulate too many conflicts?

---

_Converted to draft by @oconnor663 on 2025-08-18 14:29_

---

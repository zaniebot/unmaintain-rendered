```yaml
number: 21653
title: "[ty] uniformly weak imports"
type: pull_request
state: open
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: alex/submodule-attr-last
head: gankra/full-weak
created_at: 2025-11-27T02:17:19Z
updated_at: 2025-12-13T20:21:11Z
url: https://github.com/astral-sh/ruff/pull/21653
synced_at: 2026-01-12T15:57:30Z
```

# [ty] uniformly weak imports

---

_@Gankra_

Split out from #21583 to analyze the effect of this one change.

The code in the base PR gives `import a.b` highest priority, while `from a.b import ...` gets second-lowest priority (beating only `__getattr__`). This PR removes the special handling of `import a.b` so that they get the same priority as `from a.b import...`.

---

_Label `ty` added by @Gankra on 2025-11-27 02:17_

---

_Review requested from @carljm by @Gankra on 2025-11-27 02:17_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-27 02:17_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-27 02:17_

---

_Review requested from @sharkdp by @Gankra on 2025-11-27 02:17_

---

_Review requested from @dcreager by @Gankra on 2025-11-27 02:17_

---

_Converted to draft by @Gankra on 2025-11-27 02:17_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 02:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 02:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
PyGithub (https://github.com/PyGithub/PyGithub)
- github/Requester.py:766:19: error[call-non-callable] Object of type `<module 'github.GithubException'>` is not callable
- tests/GraphQl.py:113:32: error[invalid-argument-type] Argument to bound method `assertRaises` is incorrect: Expected `type[Unknown] | tuple[type[Unknown], ...]`, found `<module 'github.GithubException'>`
- tests/GraphQl.py:149:32: error[invalid-argument-type] Argument to bound method `assertRaises` is incorrect: Expected `type[Unknown] | tuple[type[Unknown], ...]`, found `<module 'github.GithubException'>`
- Found 262 diagnostics
+ Found 259 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/dask/test_dask_not_installed.py:47:22: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing`
+ tests/dask/test_dask_not_installed.py:47:22: warning[possibly-missing-attribute] Attribute `DataFrame` may be missing on object of type `<module 'pandera.typing'> | <module 'pandera.typing'>`
+ tests/pandas/test_pandas_accessor.py:68:13: warning[possibly-missing-attribute] Member `api` may be missing on module `pandera`
+ tests/pyspark/test_pyspark_check.py:48:10: warning[possibly-missing-attribute] Member `extensions` may be missing on module `pandera`
- Found 1601 diagnostics
+ Found 1603 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/argv.py:55:15: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/asm.py:26:31: error[invalid-argument-type] Argument to function `asm` is incorrect: Expected `ArchDefinition`, found `<module 'pwndbg.aglib.arch'>`
- pwndbg/aglib/ctypes.py:28:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/disasm/aarch64.py:338:44: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/aarch64.py:414:30: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/arch.py:156:40: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
+ pwndbg/aglib/disasm/arch.py:156:13: error[unresolved-attribute] Object of type `None` has no attribute `current`
+ pwndbg/aglib/disasm/arch.py:207:17: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/aglib/disasm/arch.py:351:40: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/arch.py:390:39: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
+ pwndbg/aglib/disasm/arch.py:405:39: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/aglib/disasm/arch.py:457:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/disasm/arch.py:457:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/disasm/arch.py:501:38: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/arch.py:528:54: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/arch.py:550:54: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/arch.py:565:36: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/arch.py:653:12: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `syscall_abi`
- pwndbg/aglib/disasm/arch.py:655:17: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/disasm/arch.py:655:41: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `syscall_abi`
- pwndbg/aglib/disasm/arch.py:734:40: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/arch.py:790:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/arch.py:800:38: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/arch.py:877:27: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
+ pwndbg/aglib/disasm/arch.py:919:31: error[unresolved-attribute] Object of type `None` has no attribute `flags`
- pwndbg/aglib/disasm/arm.py:246:23: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/disasm/arm.py:246:23: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/disasm/arm.py:309:16: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/disasm/arm.py:329:21: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/disasm/arm.py:329:21: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/disasm/disassembly.py:125:31: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `constant_instruction_size`
- pwndbg/aglib/disasm/disassembly.py:127:27: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `max_instruction_size`
- pwndbg/aglib/disasm/disassembly.py:146:13: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `get_capstone_endianness`
- pwndbg/aglib/disasm/disassembly.py:178:15: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `get_capstone_constants`
- pwndbg/aglib/disasm/disassembly.py:186:46: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `max_instruction_size`
- pwndbg/aglib/disasm/disassembly.py:382:26: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/disasm/disassembly.py:566:9: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/disasm/disassembly.py:572:16: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/disasm/instruction.py:694:40: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/aglib/disasm/instruction.py:694:40: error[invalid-assignment] Object of type `DisassembledInstruction | None` is not assignable to `DisassembledInstruction`
+ pwndbg/aglib/disasm/instruction.py:694:40: warning[possibly-missing-attribute] Attribute `disasm` may be missing on object of type `Process | None`
- pwndbg/aglib/disasm/riscv.py:143:17: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/riscv.py:209:57: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/riscv.py:210:57: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/riscv.py:248:19: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/disasm/x86.py:209:68: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/aglib/disasm/x86.py:209:68: error[unresolved-attribute] Object of type `None` has no attribute `sp`
+ pwndbg/aglib/disasm/x86.py:280:33: error[unresolved-attribute] Object of type `None` has no attribute `fsbase`
+ pwndbg/aglib/disasm/x86.py:283:33: error[unresolved-attribute] Object of type `None` has no attribute `gsbase`
+ pwndbg/aglib/disasm/x86.py:315:35: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/aglib/disasm/x86.py:321:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/aglib/disasm/x86.py:321:20: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/aglib/disasm/x86.py:321:45: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/disasm/x86.py:431:21: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/disasm/x86.py:431:21: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/elf.py:83:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/godbg.py:67:17: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/godbg.py:70:7: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/godbg.py:93:17: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/godbg.py:96:7: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/godbg.py:258:43: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/godbg.py:270:21: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
+ pwndbg/aglib/heap/mallocng.py:90:52: error[invalid-argument-type] Argument to bound method `unpack` is incorrect: Expected `bytes`, found `bytearray`
- pwndbg/aglib/heap/mallocng.py:89:39: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:90:27: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `unpack`
- pwndbg/aglib/heap/mallocng.py:90:59: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:111:54: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:204:57: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/mallocng.py:206:57: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/mallocng.py:240:62: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/mallocng.py:242:35: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/mallocng.py:651:19: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:652:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/mallocng.py:701:64: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:712:63: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:725:56: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:736:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:749:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:762:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:774:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:786:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:886:37: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:913:19: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:915:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/mallocng.py:1005:19: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/mallocng.py:1010:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/structs.py:25:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/structs.py:33:11: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/structs.py:34:11: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/structs.py:35:4: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/heap/structs.py:53:4: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/heap/structs.py:96:4: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/aglib/heap/structs.py:1069:31: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/kernel/__init__.py:218:12: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `idt`
+ pwndbg/aglib/kernel/__init__.py:218:12: error[unresolved-attribute] Object of type `None` has no attribute `idt`
- pwndbg/aglib/kernel/__init__.py:219:13: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `idt_limit`
+ pwndbg/aglib/kernel/__init__.py:219:13: error[unresolved-attribute] Object of type `None` has no attribute `idt_limit`
- pwndbg/aglib/kernel/__init__.py:221:12: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/kernel/__init__.py:345:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/kernel/__init__.py:345:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/kernel/__init__.py:460:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/kernel/__init__.py:460:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/kernel/__init__.py:465:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:467:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:474:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:476:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:478:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:485:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:487:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:640:17: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/__init__.py:652:17: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/kernel/__init__.py:652:17: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/kernel/vmmap.py:176:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/kernel/vmmap.py:176:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/kernel/vmmap.py:209:12: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:221:24: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:356:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:358:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:364:24: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:405:34: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:428:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/kernel/vmmap.py:439:43: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
+ pwndbg/aglib/memory.py:193:37: error[invalid-argument-type] Argument to bound method `unpack` is incorrect: Expected `bytes`, found `bytearray`
- pwndbg/aglib/memory.py:193:12: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `unpack`
- pwndbg/aglib/memory.py:193:48: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/memory.py:236:16: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/memory.py:332:23: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/memory.py:333:24: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/nearpc.py:118:14: error[invalid-assignment] Object of type `int | None` is not assignable to `int`
+ pwndbg/aglib/nearpc.py:118:14: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/aglib/nearpc.py:241:32: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/nearpc.py:241:32: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/aglib/next.py:33:51: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:58:51: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:80:19: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:128:27: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:211:31: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:224:30: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:226:34: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:255:10: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:273:25: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/aglib/next.py:280:26: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/aglib/objc.py:492:11: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/objc.py:1072:12: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/onegadget.py:146:24: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/onegadget.py:316:28: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/aglib/onegadget.py:319:28: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/aglib/onegadget.py:322:24: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/aglib/onegadget.py:397:72: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/onegadget.py:399:54: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/onegadget.py:399:97: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/onegadget.py:447:72: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/onegadget.py:449:54: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/aglib/onegadget.py:449:97: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
+ pwndbg/aglib/onegadget.py:316:28: warning[possibly-missing-attribute] Attribute `evaluate_expression` may be missing on object of type `Process | None`
+ pwndbg/aglib/onegadget.py:319:28: warning[possibly-missing-attribute] Attribute `evaluate_expression` may be missing on object of type `Process | None`
+ pwndbg/aglib/onegadget.py:322:24: warning[possibly-missing-attribute] Attribute `evaluate_expression` may be missing on object of type `Process | None`
+ pwndbg/aglib/onegadget.py:397:61: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
+ pwndbg/aglib/onegadget.py:399:43: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
+ pwndbg/aglib/onegadget.py:399:86: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
+ pwndbg/aglib/onegadget.py:447:61: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
+ pwndbg/aglib/onegadget.py:449:43: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
+ pwndbg/aglib/onegadget.py:449:86: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
- pwndbg/aglib/regs.py:128:32: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:139:24: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/regs.py:168:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:171:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:175:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:180:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:184:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:188:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:192:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:196:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:200:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:204:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:208:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:212:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:216:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:271:12: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/regs.py:291:50: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/aglib/shellcode.py:33:45: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/shellcode.py:35:12: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/shellcode.py:35:12: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/shellcode.py:95:45: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/shellcode.py:107:49: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `instruction_alignment`
- pwndbg/aglib/shellcode.py:110:37: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `instruction_alignment`
- pwndbg/aglib/shellcode.py:114:9: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `write_reg`
+ pwndbg/aglib/shellcode.py:114:9: error[unresolved-attribute] Object of type `None` has no attribute `write_reg`
- pwndbg/aglib/shellcode.py:120:9: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `write_reg`
+ pwndbg/aglib/shellcode.py:120:9: error[unresolved-attribute] Object of type `None` has no attribute `write_reg`
- pwndbg/aglib/shellcode.py:122:13: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `write_reg`
+ pwndbg/aglib/shellcode.py:122:13: error[unresolved-attribute] Object of type `None` has no attribute `write_reg`
- pwndbg/aglib/stack.py:76:17: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/aglib/stack.py:76:17: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/aglib/tls.py:40:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/tls.py:57:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/tls.py:63:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/tls.py:79:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/tls.py:81:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
+ pwndbg/aglib/tls.py:80:20: error[unresolved-attribute] Object of type `None` has no attribute `fsbase`
- pwndbg/aglib/tls.py:83:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
+ pwndbg/aglib/tls.py:82:20: error[unresolved-attribute] Object of type `None` has no attribute `gsbase`
- pwndbg/aglib/tls.py:86:13: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/tls.py:86:13: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/tls.py:86:52: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/tls.py:86:52: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/tls.py:88:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/tls.py:94:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/tls.py:94:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/tls.py:95:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/aglib/tls.py:96:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/aglib/tls.py:96:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/aglib/vmmap.py:25:9: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `platform`
- pwndbg/arguments.py:51:15: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `function_abi`
- pwndbg/arguments.py:67:15: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `syscall_abi`
+ pwndbg/arguments.py:47:31: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/arguments.py:88:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `platform`
- pwndbg/arguments.py:155:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `function_abi`
- pwndbg/arguments.py:158:67: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `function_abi`
- pwndbg/arguments.py:163:16: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg_uncached`
+ pwndbg/arguments.py:163:16: error[unresolved-attribute] Object of type `None` has no attribute `read_reg_uncached`
- pwndbg/arguments.py:167:10: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/arguments.py:167:10: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/arguments.py:167:38: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/arguments.py:177:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `function_abi`
- pwndbg/auxv.py:76:8: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/auxv.py:78:10: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/auxv.py:155:10: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/auxv.py:155:10: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/auxv.py:218:50: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/auxv.py:219:50: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/chain.py:89:24: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
+ pwndbg/color/disasm.py:41:54: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/__init__.py:73:24: error[unresolved-attribute] Module `pwndbg.dbg` has no member `commands`
- pwndbg/commands/__init__.py:79:4: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
+ pwndbg/commands/__init__.py:208:52: error[invalid-argument-type] Argument to bound method `add_command` is incorrect: Expected `str`, found `Unknown | str | None`
- pwndbg/commands/__init__.py:208:29: error[unresolved-attribute] Module `pwndbg.dbg` has no member `add_command`
- pwndbg/commands/__init__.py:211:33: error[unresolved-attribute] Module `pwndbg.dbg` has no member `add_command`
- pwndbg/commands/__init__.py:319:16: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/commands/__init__.py:325:24: error[unresolved-attribute] Module `pwndbg.dbg` has no member `lex_args`
- pwndbg/commands/__init__.py:357:21: error[unresolved-attribute] Module `pwndbg.dbg` has no member `history`
+ pwndbg/commands/__init__.py:493:61: error[invalid-assignment] Object of type `(Frame & ~AlwaysFalsy) | Process | None` is not assignable to `Frame | Process`
- pwndbg/commands/__init__.py:442:48: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:446:55: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:451:51: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
+ pwndbg/commands/__init__.py:514:15: error[unresolved-attribute] Object of type `None` has no attribute `fix`
- pwndbg/commands/__init__.py:455:55: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:492:13: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
- pwndbg/commands/__init__.py:494:29: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/commands/__init__.py:784:26: warning[possibly-missing-attribute] Attribute `is_dynamically_linked` may be missing on object of type `Process | None`
- pwndbg/commands/__init__.py:592:12: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
- pwndbg/commands/__init__.py:784:26: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/commands/__init__.py:858:61: error[invalid-assignment] Object of type `(Frame & ~AlwaysFalsy) | Process | None` is not assignable to `Frame | Process`
- pwndbg/commands/__init__.py:857:13: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
- pwndbg/commands/__init__.py:859:29: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/commands/__init__.py:899:8: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
+ pwndbg/commands/__init__.py:866:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Value | None`
- pwndbg/commands/ai.py:200:9: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/ai.py:200:9: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/commands/aslr.py:15:4: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
- pwndbg/commands/aslr.py:46:12: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
- pwndbg/commands/aslr.py:78:12: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
+ pwndbg/commands/binja.py:21:42: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/binja_functions.py:81:38: error[invalid-argument-type] Argument to function `l2r` is incorrect: Expected `int`, found `int | None`
+ pwndbg/commands/binja_functions.py:81:38: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/binja_functions.py:89:12: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/binja_functions.py:89:12: error[unresolved-attribute] Object of type `None` has no attribute `sp`
+ pwndbg/commands/binja_functions.py:133:14: error[unresolved-attribute] Object of type `None` has no attribute `current`
- pwndbg/commands/binja_functions.py:134:13: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/binja_functions.py:134:13: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/commands/context.py:942:24: error[unresolved-attribute] Object of type `None` has no attribute `changed`
- pwndbg/commands/context.py:960:15: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/context.py:960:15: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/commands/context.py:971:47: error[unresolved-attribute] Object of type `None` has no attribute `last`
+ pwndbg/commands/context.py:1023:17: error[unresolved-attribute] Object of type `None` has no attribute `gpr`
+ pwndbg/commands/context.py:1025:21: error[unresolved-attribute] Object of type `None` has no attribute `frame`
+ pwndbg/commands/context.py:1026:21: error[unresolved-attribute] Object of type `None` has no attribute `stack`
+ pwndbg/commands/context.py:1029:21: error[unresolved-attribute] Object of type `None` has no attribute `retaddr`
+ pwndbg/commands/context.py:1031:21: error[unresolved-attribute] Object of type `None` has no attribute `current`
- pwndbg/commands/context.py:1033:51: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `kernel`
+ pwndbg/commands/context.py:1033:51: error[unresolved-attribute] Object of type `None` has no attribute `kernel`
- pwndbg/commands/context.py:1034:24: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `kernel`
+ pwndbg/commands/context.py:1034:24: error[unresolved-attribute] Object of type `None` has no attribute `kernel`
- pwndbg/commands/context.py:1039:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `kernel`
+ pwndbg/commands/context.py:1039:20: error[unresolved-attribute] Object of type `None` has no attribute `kernel`
+ pwndbg/commands/context.py:1045:21: error[unresolved-attribute] Object of type `None` has no attribute `flags`
- pwndbg/commands/context.py:1050:51: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `kernel`
+ pwndbg/commands/context.py:1050:51: error[unresolved-attribute] Object of type `None` has no attribute `kernel`
- pwndbg/commands/context.py:1051:16: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `kernel`
+ pwndbg/commands/context.py:1051:16: error[unresolved-attribute] Object of type `None` has no attribute `kernel`
- pwndbg/commands/context.py:1052:29: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `kernel`
+ pwndbg/commands/context.py:1052:29: error[unresolved-attribute] Object of type `None` has no attribute `kernel`
+ pwndbg/commands/context.py:1065:15: error[unresolved-attribute] Object of type `None` has no attribute `current`
- pwndbg/commands/context.py:1121:13: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/context.py:1124:48: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/context.py:1236:54: error[invalid-argument-type] Argument to bound method `decompile` is incorrect: Expected `int`, found `int | None`
+ pwndbg/commands/context.py:1236:54: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/context.py:1256:9: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/context.py:1256:9: error[unresolved-attribute] Object of type `None` has no attribute `sp`
+ pwndbg/commands/context.py:1458:41: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/cpsr.py:27:21: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
+ pwndbg/commands/cpsr.py:28:17: error[unresolved-attribute] Object of type `None` has no attribute `flags`
- pwndbg/commands/cpsr.py:33:19: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/cpsr.py:33:19: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/commands/cyclic.py:31:16: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/cyclic.py:32:14: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/commands/cyclic.py:40:25: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/cyclic.py:161:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/cyclic.py:171:51: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
- pwndbg/commands/cymbol.py:100:51: error[invalid-argument-type] Argument to function `flags` is incorrect: Expected `ArchDefinition`, found `<module 'pwndbg.aglib.arch'>`
- pwndbg/commands/distance.py:36:29: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/distance.py:41:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/distance.py:42:22: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/distance.py:48:44: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/dumpargs.py:30:30: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/dumpargs.py:30:56: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
+ pwndbg/commands/flags.py:40:20: error[unresolved-attribute] Object of type `None` has no attribute `current`
- pwndbg/commands/flags.py:59:31: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/flags.py:59:31: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/commands/flags.py:66:17: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `write_reg`
+ pwndbg/commands/flags.py:66:17: error[unresolved-attribute] Object of type `None` has no attribute `write_reg`
- pwndbg/commands/got.py:171:20: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/hexdump.py:107:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/hexdump.py:108:33: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/hexdump.py:142:62: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
+ pwndbg/commands/ida.py:32:18: warning[possibly-missing-attribute] Attribute `pc` may be missing on object of type `Frame | None`
- pwndbg/commands/ida.py:25:2: error[unresolved-attribute] Module `pwndbg.dbg` has no member `event_handler`
- pwndbg/commands/ida.py:32:18: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
- pwndbg/commands/ida.py:142:14: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
+ pwndbg/commands/ida.py:142:14: warning[possibly-missing-attribute] Attribute `pc` may be missing on object of type `Frame | None`
+ pwndbg/commands/ida.py:165:36: error[unresolved-attribute] Object of type `None` has no attribute `frame`
- pwndbg/commands/ida.py:166:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/ida.py:166:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/commands/ida.py:166:47: error[unresolved-attribute] Object of type `None` has no attribute `frame`
- pwndbg/commands/ida.py:167:16: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/ida.py:167:16: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/commands/ida.py:167:43: error[unresolved-attribute] Object of type `None` has no attribute `stack`
- pwndbg/commands/integration.py:40:56: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `int | None`
+ pwndbg/commands/integration.py:37:16: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/kbase.py:50:9: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/commands/kbase.py:50:9: warning[possibly-missing-attribute] Attribute `add_symbol_file` may be missing on object of type `Process | None`
+ pwndbg/commands/kmem_trace.py:181:16: error[unresolved-attribute] Object of type `None` has no attribute `retval`
- pwndbg/commands/kmem_trace.py:184:19: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg_uncached`
+ pwndbg/commands/kmem_trace.py:184:19: error[unresolved-attribute] Object of type `None` has no attribute `read_reg_uncached`
+ pwndbg/commands/kmem_trace.py:184:55: error[unresolved-attribute] Object of type `None` has no attribute `retval`
+ pwndbg/commands/kmem_trace.py:202:16: error[unresolved-attribute] Object of type `None` has no attribute `retval`
- pwndbg/commands/kmem_trace.py:205:16: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg_uncached`
+ pwndbg/commands/kmem_trace.py:205:16: error[unresolved-attribute] Object of type `None` has no attribute `read_reg_uncached`
+ pwndbg/commands/kmem_trace.py:205:52: error[unresolved-attribute] Object of type `None` has no attribute `retval`
+ pwndbg/commands/kmem_trace.py:282:8: error[unresolved-attribute] Object of type `None` has no attribute `retval`
- pwndbg/commands/misc.py:36:20: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
- pwndbg/commands/misc.py:55:13: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
+ pwndbg/commands/misc.py:36:20: warning[possibly-missing-attribute] Attribute `evaluate_expression` may be missing on object of type `Frame | None`
+ pwndbg/commands/misc.py:55:13: warning[possibly-missing-attribute] Attribute `evaluate_expression` may be missing on object of type `Frame | None`
- pwndbg/commands/p2p.py:45:20: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/plist.py:202:17: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
+ pwndbg/commands/plist.py:202:17: warning[possibly-missing-attribute] Attribute `evaluate_expression` may be missing on object of type `Frame | None`
- pwndbg/commands/plist.py:284:39: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/plist.py:436:65: error[invalid-argument-type] Argument to function `get_typed_pointer_value` is incorrect: Expected `str | Type`, found `Unknown | None`
+ pwndbg/commands/plist.py:436:65: error[invalid-argument-type] Argument to function `get_typed_pointer_value` is incorrect: Expected `str | Type`, found `Type | None`
- pwndbg/commands/probeleak.py:93:16: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/probeleak.py:94:15: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/probeleak.py:126:13: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `unpack`
+ pwndbg/commands/radare2.py:48:16: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/radare2.py:55:35: error[invalid-argument-type] Argument to function `hex` is incorrect: Expected `SupportsIndex`, found `int | None | Unknown`
- pwndbg/commands/retaddr.py:21:10: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/retaddr.py:21:10: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/commands/retaddr.py:35:15: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
+ pwndbg/commands/rizin.py:46:16: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/rizin.py:53:35: error[invalid-argument-type] Argument to function `hex` is incorrect: Expected `SupportsIndex`, found `int | None | Unknown`
- pwndbg/commands/rop.py:27:36: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `get_capstone_constants`
- pwndbg/commands/rop.py:116:48: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/search.py:201:41: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/search.py:225:18: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/search.py:226:43: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `endian`
+ pwndbg/commands/segments.py:18:19: error[unresolved-attribute] Object of type `None` has no attribute `fsbase`
+ pwndbg/commands/segments.py:30:19: error[unresolved-attribute] Object of type `None` has no attribute `gsbase`
+ pwndbg/commands/sigreturn.py:92:42: error[invalid-argument-type] Argument to bound method `unpack` is incorrect: Expected `bytes`, found `bytearray`
- pwndbg/commands/sigreturn.py:74:16: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
- pwndbg/commands/sigreturn.py:76:44: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/sigreturn.py:77:46: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `name`
- pwndbg/commands/sigreturn.py:92:17: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `unpack`
+ pwndbg/commands/sigreturn.py:99:21: error[unresolved-attribute] Object of type `None` has no attribute `flags`
+ pwndbg/commands/sigreturn.py:100:25: error[unresolved-attribute] Object of type `None` has no attribute `flags`
- pwndbg/commands/start.py:24:4: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
- pwndbg/commands/start.py:34:12: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `int | None`
- pwndbg/commands/start.py:39:12: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/commands/start.py:34:12: error[unresolved-attribute] Object of type `None` has no attribute `pc`
- pwndbg/commands/start.py:136:8: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
+ pwndbg/commands/start.py:40:10: warning[possibly-missing-attribute] Attribute `break_at` may be missing on object of type `Process | None`
+ pwndbg/commands/start.py:46:5: warning[possibly-missing-attribute] Attribute `dispatch_execution_controller` may be missing on object of type `Process | None`
- pwndbg/commands/telescope.py:120:39: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/telescope.py:120:39: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/commands/telescope.py:125:30: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/telescope.py:127:34: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrmask`
- pwndbg/commands/telescope.py:134:19: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/telescope.py:134:19: error[unresolved-attribute] Object of type `None` has no attribute `sp`
+ pwndbg/commands/telescope.py:142:16: error[unresolved-attribute] Object of type `None` has no attribute `frame`
- pwndbg/commands/telescope.py:145:14: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/telescope.py:145:14: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/commands/telescope.py:146:14: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/telescope.py:146:14: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/commands/telescope.py:146:41: error[unresolved-attribute] Object of type `None` has no attribute `frame`
+ pwndbg/commands/telescope.py:171:16: error[unresolved-attribute] Object of type `None` has no attribute `common`
- pwndbg/commands/telescope.py:172:20: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/telescope.py:172:20: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
- pwndbg/commands/telescope.py:189:31: error[unresolved-attribute] Module `pwndbg.aglib.arch` has no member `ptrsize`
+ pwndbg/commands/telescope.py:230:38: error[unresolved-attribute] Object of type `None` has no attribute `frame`
- pwndbg/commands/telescope.py:231:14: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `read_reg`
+ pwndbg/commands/telescope.py:231:14: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
+ pwndbg/commands/telescope.py:231:41: error[unresolved-attribute] Object of type `None` has no attribute `frame`
- pwndbg/commands/telescope.py:331:17: error[unresolved-attribute] Module `pwndbg.aglib.regs` has no member `sp`
+ pwndbg/commands/telescope.py:331:17: error[unresolved-attribute] Object of type `None` has no attribute `sp`
- pwndbg/c

... (truncated 131 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 02:26_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 74 | 261 | 68 |
| `possibly-missing-attribute` | 20 | 0 | 1 |
| `invalid-argument-type` | 5 | 11 | 1 |
| `unsupported-operator` | 6 | 0 | 0 |
| `invalid-assignment` | 3 | 1 | 0 |
| `call-non-callable` | 0 | 1 | 0 |
| **Total** | **108** | **274** | **70** |

**[Full report with detailed diff](https://gankra-full-weak.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-full-weak.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-27 02:28_

Ayup, basically just rerolls where we mess up on pwndbg

---

_Comment by @Gankra on 2025-11-27 02:30_

[pwndbg explicitly shadows it's own submodule with `None` and then in an init subroutine unshadows it](https://github.com/pwndbg/pwndbg/blob/9763fc34370d0fab9ef35c36f3c6c560991dbec8/pwndbg/aglib/__init__.py#L18-L52) with explicit reference to it doing things "so mypy understands"

This is basically the entire diff. Both in added diagnostics and removed diagnostics. Very potato-potatoh situation. pwndbg is messed up either way. I guess this is the motivating case but boy I sure wish they just had an `if TYPE_CHECKING` or something instead...

---

_Comment by @Gankra on 2025-11-27 02:34_

The other changes are:

* ✅ PyGithub has a call-non-callable fixed
* 😒 One test in dd-trace-py gets more uncertain about a submodule that is explicitly imported in the file
* 😒 A few tests in pandas get more uncertain about a submodule that is explicitly imported in the file

---

_Closed by @Gankra on 2025-12-13 20:02_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-12-13 20:02_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-13 20:02_

---

_Comment by @Gankra on 2025-12-13 20:03_

Whoops, that was a bad rebase...

---

_Reopened by @Gankra on 2025-12-13 20:11_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-12-13 20:12_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-13 20:12_

---

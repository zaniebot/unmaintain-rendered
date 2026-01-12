```yaml
number: 893
title: Don’t use .gitignore of a different git repository
type: issue
state: closed
author: SimonSapin
labels:
  - bug
assignees: []
created_at: 2018-04-22T15:17:37Z
updated_at: 2018-04-23T22:33:32Z
url: https://github.com/BurntSushi/ripgrep/issues/893
synced_at: 2026-01-12T16:13:22Z
```

# Don’t use .gitignore of a different git repository

---

_@SimonSapin_

In a clone of https://github.com/rust-lang/rust with git submodules checked out, in the `src/llvm` submodule, ripgrep 0.8.1 seems to "miss" several results that are found by GNU grep or git-grep:

```sh
~/rust/src/llvm% rg __powisf2                   
lib/Target/WebAssembly/WebAssemblyRuntimeLibcallSignatures.cpp
643:/* POWI_F32 */ "__powisf2",
```
```sh
~/rust/src/llvm% grep -R __powisf2            
include/llvm/CodeGen/RuntimeLibcalls.def:HANDLE_LIBCALL(POWI_F32, "__powisf2")
test/CodeGen/Mips/msa/f16-llvm-ir.ll:; MIPS32-DAG:     lw $25, %call16(__powisf2)($gp)
test/CodeGen/Mips/msa/f16-llvm-ir.ll:; MIPS64-N32-DAG: lw $25, %call16(__powisf2)($gp)
test/CodeGen/Mips/msa/f16-llvm-ir.ll:; MIPS64-N64-DAG: ld $25, %call16(__powisf2)($gp)
test/CodeGen/Mips/pr36061.ll:; MIPSN64-NEXT:    jal __powisf2
test/CodeGen/Mips/pr36061.ll:; MIPSN32-NEXT:    jal __powisf2
test/CodeGen/AArch64/illegal-float-ops.ll:; CHECK: bl __powisf2
test/CodeGen/AArch64/f16-instructions.ll:; CHECK-COMMON-NEXT: bl {{_?}}__powisf2
test/CodeGen/AArch64/arm64-illegal-float-ops.ll:; CHECK: bl __powisf2
test/CodeGen/SystemZ/fp-libcall.ll:; CHECK: brasl %r14, __powisf2@PLT
test/CodeGen/Thumb2/intrinsics-cc.ll:; CHECK: b __powisf2
test/CodeGen/Thumb2/float-intrinsics-float.ll:; SOFT: bl __powisf2
test/CodeGen/Thumb2/float-intrinsics-float.ll:; HARD: b __powisf2
test/CodeGen/ARM/Windows/powi.ll:; CHECK-NOT: __powisf2
test/CodeGen/ARM/Windows/powi.ll:; CHECK-NOT: __powisf2
test/CodeGen/ARM/Windows/powi.ll:; CHECK-NOT: bl __powisf2
test/CodeGen/ARM/fp16-promote.ll:; CHECK-FP16: bl __powisf2
test/CodeGen/ARM/fp16-promote.ll:; CHECK-LIBCALL: bl __powisf2
test/CodeGen/XCore/float-intrinsics.ll:; CHECK: bl __powisf2
lib/Target/WebAssembly/WebAssemblyRuntimeLibcallSignatures.cpp:/* POWI_F32 */ "__powisf2",
```

This turns out to be because of the `.gitignore` file at the root of the Rust repository. Running with `--no-ignore-parent` makes it find those extra results.

These files are not ignored by git. If I modify `~/rust/src/llvm/include/llvm/CodeGen/RuntimeLibcalls.def` for example, `git status` will show it as modified. My understanding is that `~/rust/.gitignore` is not relevant to `RuntimeLibcalls.def` because it belongs to a different repository. This is detected by the presence of a `.git` directory ([or file](https://git-scm.com/docs/git-worktree)) at `~/rust/src/llvm/.git`.

When going up the parent/ancestor directories, ripgrep should probably stop considering `.gitignore` files after it goes beyond the root of a git repository (as indicated by `.git`). Conversely, when descending into a directory that contains `.git`, it should maybe stop considering `.gitignore` files that are "above" this directory.

---

_Comment by @BurntSushi on 2018-04-22 15:51_

Ah this is a great bug! ripgrep does actually know to stop respecting parent `.gitignore` once it crosses a repository boundary. The problem is that it's looking for a `.git` *directory*:

https://github.com/BurntSushi/ripgrep/blob/6b15ce2342d76ceccc16fad7614eada218a663da/ignore/src/dir.rs#L257

But it looks like `.git` can also be a file, which ripgrep should obviously respect. I think fixing that should completely fix this.

---

_Label `bug` added by @BurntSushi on 2018-04-22 15:52_

---

_Comment by @SimonSapin on 2018-04-22 16:29_

Indeed, it looks like `.git` is a file for submodules too. I hadn’t realized that.

---

_Closed by @BurntSushi on 2018-04-23 22:33_

---

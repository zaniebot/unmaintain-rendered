```yaml
number: 2948
title: ripgrep returning no results if you mention a directory that is .gitignored
type: issue
state: closed
author: jayfoad
labels: []
assignees: []
created_at: 2024-12-16T15:41:20Z
updated_at: 2025-07-30T08:21:07Z
url: https://github.com/BurntSushi/ripgrep/issues/2948
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep returning no results if you mention a directory that is .gitignored

---

_@jayfoad_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1 (rev 79cbe89deb)

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.

### How did you install ripgrep?

Built from source with `cargo build --release`

### What operating system are you using ripgrep on?

Ubuntu 24.04.1 LTS

### Describe your bug.

In a particular LLVM checkout, this search returns some results:
`rg ApplyCallback a/ b/ benchmarks/ bindings/ c/ cmake/ docs/ examples/ include/ lib/ projects/ resources/`
But this search returns no results:
`rg ApplyCallback a/ b/ benchmarks/ bindings/ c/ cmake/ docs/ examples/ include/ lib/ projects/ resources/ runtimes/`
Since I have only specified one *extra* directory to search, I would not expect fewer results.

### What are the steps to reproduce the behavior?

```sh
git clone https://github.com/llvm/llvm-project.git
cd llvm-project/llvm
mkdir a b c
rg ApplyCallback */
```

### What is the actual behavior?

The `rg` command above returns no results.

### What is the expected behavior?

The command should return some results, because the search string occurs in e.g. `include/llvm/TableGen/TableGenBackend.h`.

---

_Comment by @jayfoad on 2024-12-16 15:44_

Note that `llvm-project/llvm/.gitignore` contains:
```
runtimes/*
!runtimes/*.*
```

I have seen some related issues like #2498 and #1050 but this one seems to be slightly different.

---

_Comment by @ADITI-MISHRA310 on 2025-07-29 23:33_

Its showing the expected output. 

---

_Comment by @jayfoad on 2025-07-30 08:21_

I can no longer reproduce it.

---

_Closed by @jayfoad on 2025-07-30 08:21_

---

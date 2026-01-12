```yaml
number: 2990
title: "Globs that end with a dot don't work properly"
type: issue
state: closed
author: tavianator
labels:
  - bug
  - rollup
assignees: []
created_at: 2025-02-14T17:06:10Z
updated_at: 2025-09-20T01:08:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2990
synced_at: 2026-01-12T16:13:25Z
```

# Globs that end with a dot don't work properly

---

_@tavianator_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

### How did you install ripgrep?

pacman

### What operating system are you using ripgrep on?

Arch Linux

### Describe your bug.

Ripgrep seems to ignore glob patterns that end with a dot (`.`)

### What are the steps to reproduce the behavior?

```console
tavianator@graphene $ mkdir asdf asdf.
tavianator@graphene $ touch asdf/foo asdf./foo
```

### What is the actual behavior?

```console
tavianator@graphene $ rg --files -g '!asdf/'
asdf./foo
tavianator@graphene $ rg --files -g '!asdf./'
asdf./foo
asdf/foo
```

### What is the expected behavior?

```console
tavianator@graphene $ rg --files -g '!asdf./'
asdf/foo
```

---

_Label `bug` added by @BurntSushi on 2025-08-17 21:31_

---

_Label `rollup` added by @BurntSushi on 2025-08-17 21:31_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

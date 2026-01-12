```yaml
number: 2747
title: Incorrect application of ignore rule with single glob in nested 
type: issue
state: closed
author: tmccombs
labels:
  - bug
  - rollup
assignees: []
created_at: 2024-03-06T07:48:43Z
updated_at: 2025-09-20T01:08:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2747
synced_at: 2026-01-12T16:13:24Z
```

# Incorrect application of ignore rule with single glob in nested 

---

_@tmccombs_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

pacman

### What operating system are you using ripgrep on?

archlinux

### Describe your bug.

If you have an ignore rule like "/a/*/b" in .ignore or .gitignore,  then it will ignore files it shouldn't if you are in a subdirectory that has a grandchild named b. 

See also https://github.com/sharkdp/fd/issues/1506

### What are the steps to reproduce the behavior?

Create a directory with a layout like:
```
.
└── a
    ├── c
    │   └── b
    │       └── foo
    └── src
        └── f
            └── b
                └── foo

7 directories, 2 files
```
and at the top level create a .ignore file with the folowing contents:
```
/a/*/b
```

From the root run `rg --files`
then run `rg --files` from the a/src directory (either cd into that directory or supply it as the path)

### What is the actual behavior?

When run from the root folder, it works as expected and finds a/src/f/b/foo but not a/c/b/foo (because `*` only matches a single directory). 

However, if I run from `a/src` then it doesn't find any files. 

Possibly related, if I run `rg --files` from the `a/` folder, then it actually finds both foo files, even though a/c/b/foo _should_ be excluded because of it's relation to the folder containing the .ignore file (and the ignore pattern is absolute).

### What is the expected behavior?

a/src/f/b/foo should not be ignored, regardless of which directory I run `rg` from. 

a/c/b/foo should be ignored, regardless of which directory I run `rg` from.

---

_Label `bug` added by @BurntSushi on 2024-03-06 12:16_

---

_Comment by @em9797b on 2024-03-07 02:44_


Where it goes wrong:

https://github.com/BurntSushi/ripgrep/blob/master/crates/ignore/src/dir.rs#L456

In the specific test case, we have:

```
abs_parent_path = "[...]/a/src'
dirpath = "./f/b"
```

Prefix is computed to be `f` and that and the `/` are stripped before the base is concatenated with the absolute path giving:
```
"[...]/a/src/b"
```
So that matches on the ignore regex and is filtered. The obvious (to me) solution seemed to be to strip the "./" (if necessary) and just do plain concatenation. We would expect our path not to have overlap its absolute parent, right? However, making that change causes regression of #1757. So, either get we doubled-up segments or segments missing.

> Possibly related, if I run rg --files from the a/ folder, then it actually finds both foo files, even though a/c/b/foo should be excluded because of it's relation to the folder containing the .ignore file (and the ignore pattern is absolute).

They are related! The same sequence of events results in the `b` being sliced out of `a/c/b/foo`.

---

_Comment by @WalterScottYoung on 2024-11-15 12:14_

i think this is essentially the same bug as https://github.com/BurntSushi/ripgrep/issues/2836, and I tested https://github.com/BurntSushi/ripgrep/pull/2933 is able to fix this.


---

_Label `rollup` added by @BurntSushi on 2025-07-11 16:32_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

```yaml
number: 2984
title: "regex parse error for \"\\[[^[]*\\*[^[]*\\]\""
type: issue
state: closed
author: limusch
labels: []
assignees: []
created_at: 2025-02-04T16:11:33Z
updated_at: 2025-02-04T16:30:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2984
synced_at: 2026-01-12T16:13:25Z
```

# regex parse error for "\[[^[]*\*[^[]*\]"

---

_@limusch_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep version: 14.1.1 on ArchLinux
```
rg --version
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

```

### How did you install ripgrep?

pacman -S ripgrep

### What operating system are you using ripgrep on?

ArchLinux updated today

### Describe your bug.

try to search for array definitions in C code where a '*' is used inside the brackets.
For example:
`char stringbuf[12*128];`
should match but not matching text like this: ` "element[3]", val * len[3]`


### What are the steps to reproduce the behavior?

command:
```
rg "\[[^[]*\*[^[]*\]"
rg: regex parse error:
    (?:\[[^[]*\*[^[]*\])
                  ^^
error: unclosed character class

```
This regex works with standard Linux grep or ag commands.


### What is the actual behavior?

see above

### What is the expected behavior?

actually search and find the proper matches

---

_Locked by @ghost on 2025-02-04 16:30_

---

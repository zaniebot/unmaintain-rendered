```yaml
number: 2659
title: fish completions invalid arguments
type: issue
state: closed
author: bomgar
labels:
  - bug
  - rollup
assignees: []
created_at: 2023-11-27T14:50:50Z
updated_at: 2023-11-28T02:17:16Z
url: https://github.com/BurntSushi/ripgrep/issues/2659
synced_at: 2026-01-12T16:13:24Z
```

# fish completions invalid arguments

---

_@bomgar_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.1

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

pacman

### What operating system are you using ripgrep on?

Linux  6.6.2-arch1-1

### Describe your bug.

```
/usr/share/fish/vendor_completions.d/rg.fish (line 6):
complete -c rg -n '__fish_use_subcommand'  no-binary -d 'Search binary files.'
^
from sourcing file /usr/share/fish/vendor_completions.d/rg.fish

(Type 'help complete' for related documentation)
complete: too many arguments

/usr/share/fish/vendor_completions.d/rg.fish (line 8):
complete -c rg -n '__fish_use_subcommand'  no-block-buffered -d 'Force block buffering.'
^
from sourcing file /usr/share/fish/vendor_completions.d/rg.fish

(Type 'help complete' for related documentation)
complete: too many arguments

/usr/share/fish/vendor_completions.d/rg.fish (line 10):
complete -c rg -n '__fish_use_subcommand'  no-byte-offset -d 'Print the byte offset for each matching line.'
```

I get this when I try to use the completions.



I tried to reproduce this in my shell:

This is from the completions file
```
» complete -c rg -n '__fish_use_subcommand'  no-binary -d 'Search binary files.'
complete: too many arguments
```

Adding `-l` helps. This would work:
```
complete -c rg -n '__fish_use_subcommand' -l no-binary -d 'Search binary files.'
```

### What are the steps to reproduce the behavior?

Execute some of the completion file for fish in the shell.

For example: `complete -c rg -n '__fish_use_subcommand'  no-binary -d 'Search binary files.'`

### What is the actual behavior?

Ripgrep itself was not executed

### What is the expected behavior?

Completion commands don't cause errors.

---

_Comment by @pfactum on 2023-11-27 16:44_

It looks like there's also another PR aimed at this very issue: #2655

---

_Label `bug` added by @BurntSushi on 2023-11-28 00:59_

---

_Label `rollup` added by @BurntSushi on 2023-11-28 01:01_

---

_Closed by @BurntSushi on 2023-11-28 02:17_

---

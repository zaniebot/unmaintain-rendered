```yaml
number: 2750
title: "rg allocates too much memory with: `rg --files --ignore-file ~/.ultimate-gitignore`"
type: issue
state: closed
author: wis
labels:
  - bug
  - rollup
assignees: []
created_at: 2024-03-07T23:49:03Z
updated_at: 2025-10-11T00:13:32Z
url: https://github.com/BurntSushi/ripgrep/issues/2750
synced_at: 2026-01-12T16:13:24Z
```

# rg allocates too much memory with: `rg --files --ignore-file ~/.ultimate-gitignore`

---

_@wis_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)
```

### How did you install ripgrep?

`sudo pacman -S ripgrep` (Arch Linux package manager)

### What operating system are you using ripgrep on?

Arch Linux on WSL2
```
sh uname -a
Linux my-hostname 5.15.146.1-microsoft-standard-WSL2 #1 SMP Thu Jan 11 04:09:03 UTC 2024 x86_64 GNU/Linux
```

### Describe your bug.

ripgrep process uses 7.1 Gibibytes of memory and is *really* slow (before getting killed by OS for allocating too much RAM),
when you run `rg --files --ignore-file ~/.ultimate-gitignore`, and `.ultimate-gitignore`  being a big/huge .[gitignore file](https://github.com/BurntSushi/ripgrep/files/14531210/default.ultimate-gitignore.txt), with 16332 lines (304KB), 9298 entries (or lines [with empty lines and comments are removed](https://github.com/BurntSushi/ripgrep/files/14531205/default.ultimate-gitignore-entries-only.txt))

this "Ultimate .gitignore" file is created by running `cat * > .ultimate-gitignore` in this directory [github.com/toptal/gitignore/templates](https://github.com/toptal/gitignore/tree/0a7fb01801c62ca53ab2dcd73ab96185e159e864/templates)

### What are the steps to reproduce the behavior?

> If the corpus is too big and you cannot decrease its size, file the bug anyway and the ripgrep maintainers will help figure out next steps.

Nope, the corpus is not too big, it is only 2 files and a .git directory, I made sure to test it on a small corpus but my original use case was to use this command anywhere on disk, e.g. the `$HOME` directory 

You can download these 2 files and .git directory by cloning this repo: [wis/killall-for-Windows](https://github.com/wis/killall-for-Windows/tree/92e3296b1292731cc17c62005e651024f47662ba/)

and again, the command is: `rg --files --ignore-file ~/.ultimate-gitignore`

### What is the actual behavior?

command:
```
rg --files --ignore-file ~/.ultimate-gitignore
```

output:
```
killall.c
zsh: killed     rg --files --ignore-file ~/.gitignore
```

The command runs for 10 seconds and the process allocates 7.1 Gibibytes and has on average 50% CPU usage/utilization.

### What is the expected behavior?

List all the files in the current directory, excluding the ones that match the patterns in the `.ultimate-gitignore` file 

EDIT: I tried using `fd`, as Andrew [recommends here](https://github.com/BurntSushi/ripgrep/issues/2314#issuecomment-1261317817): 
> You could also use a purpose driven tool specifically for searching by file name: https://github.com/sharkdp/fd/

but it also suffers from the same issue, the process allocates too much memory and gets killed by the OS.

I am starting to think this issue is fundamentally unfixable, given the nature of ripgrep's (and fd's) implementation, 
I don't think you can "load" and compile this much patterns into memory and have this grantee by the regex crate:
> This implementation uses finite automata and guarantees linear time matching on all inputs.

It's either ripgrep uses, or keeps using this fast regex implementation, which seems to me to trade off memory for speed,
or it's either ripgrep would be able to run this command, or a command that has this many patterns.
It's like:
1. fast ripgrep (pattern matching)
2. able to run this command
—pick one

EDIT2: I read the section on memory in the manpage, as recommended by Andrew [in this comment here](https://github.com/BurntSushi/ripgrep/issues/1553#issuecomment-615189031):
> There are cases where ripgrep may use a lot of memory. They are documented in the man page. Please consult those to see if they match your use case.

...and I then ran the command above with the `-j1` flag, as recommended in the manpage, 
the command worked, it finished and exited sucessfully, but it was *quite* slow, even on a directory as small as the one mentioned above, with 2 files and 1 directory in it.
the command took 0.9 seconds to finish, on average, whereas `rg --files` takes 0.025 seconds (25 milliseconds) to finish, on average.

---

_Renamed from "rg process uses 7.1GiB and gets killed by OS when running `rg --files --ignore-file ~/.ultimate-gitignore`" to "rg allocates too much memory with: `rg --files --ignore-file ~/.ultimate-gitignore`" by @wis on 2024-03-07 23:53_

---

_Label `bug` added by @BurntSushi on 2024-03-08 01:01_

---

_Comment by @BurntSushi on 2024-03-08 01:01_

This appears to be a regression from ripgrep 13:

```
[andrew@duff i2750]$ time rg-13.0.0 --ignore-file .ultimate-gitignore /dev/null

real    0.143
user    0.173
sys     0.017
maxmem  49 MB
faults  0
[andrew@duff i2750]$ time rg-14.1.0 --ignore-file .ultimate-gitignore /dev/null

real    1.445
user    0.810
sys     4.252
maxmem  14238 MB
faults  52
```

My bet is that it's related to https://github.com/rust-lang/regex/issues/1116.

And that in turn is probably related to the regex engine rewrite that landed in ripgrep 14.

---

_Comment by @wis on 2024-03-08 01:27_

Ah, Thanks Andrew. It may be worth it for me to note and for you to know that:

Repeat runs of: `cd "$HOME" && time rg -j6 --files --ignore-file ~/.ultimate-gitignore.txt`
take 10.5 seconds on average (of few runs). and btw with `-j6` finishes but with `-j7` uses too much memory, gets killed and fails.

But repeat runs of:  `cd "$HOME" && time rg -j1 --files --ignore-file ~/.ultimate-gitignore.txt` (with `-j1`, not `-j6`) 
take 2.1 seconds on average (of few runs), pretty fast, but the first run out of the bunch of repeat runs is slow, as slow as with `-j6`: 10.5 seconds

That is odd. both `-j6` being slower, not faster than `-j1`, and repeat runs with `-j1` being faster than first run, (but this is not the case for repeat runs with `-j6`).

---

_Comment by @Biosias on 2025-07-16 12:23_

Hello, just want to report that this issue is still relevant.

I'm using ripgrep-14.1.1-r1

while using

```
rg -sz Clear /var/log/mail/*.zst
``` 

ripgrep fills up memory and gets killed by the OS.

---

_Comment by @BurntSushi on 2025-10-07 01:52_

I can confirm that this is fixed by https://github.com/rust-lang/regex/pull/1302. Memory usage returns to about what it was in ripgrep 13.

---

_Label `rollup` added by @BurntSushi on 2025-10-07 02:11_

---

_Closed by @BurntSushi on 2025-10-11 00:13_

---

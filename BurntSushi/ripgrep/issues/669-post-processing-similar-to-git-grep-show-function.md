```yaml
number: 669
title: "Post-processing similar to \"git grep --show-function\""
type: issue
state: closed
author: zachriggle
labels:
  - duplicate
assignees: []
created_at: 2017-11-09T21:22:52Z
updated_at: 2022-07-25T23:09:55Z
url: https://github.com/BurntSushi/ripgrep/issues/669
synced_at: 2026-01-12T16:13:22Z
```

# Post-processing similar to "git grep --show-function"

---

_@zachriggle_

(Context: I have read #282 and #236 and understand the reason for closing them.  This is not a request for scope-limited searches.)

The `git-grep` command supports the `--show-function` argument, which precedes any matches with the function (or struct, or other language-dependent block) they are contained within.

For example:

```
$ git grep -p files_with_matches
src/args.rs=34=pub struct Args {
src/args.rs:44:    files_with_matches: bool,
src/args.rs=81=impl Args {
src/args.rs:159:            && !self.files_with_matches
src/args.rs:218:            .files_with_matches(self.files_with_matches)
src/args.rs=305=impl<'a> ArgMatches<'a> {
src/args.rs:325:            files_with_matches: self.is_present("files-with-matches"
...
```

The closest mechanism is a wrapper script that uses `rg` to find matching files, and uses `git-grep` to re-search the matching files so that functions are printed.

```
#!/bin/sh
rg -l "$@" | xargs git grep -p "$@"
```

When searching the Linux Kernel, we can see that `rg` is way faster (as expected) and used as an `xargs` filter even speeds up `git-grep` significantly.  However, this is slower than `rg` (as expected) and loses the styling of `rg`.

We can also see that the overhead of printing out the function context is near-negligible (it was occasionally faster than not printing function context, which is odd).

```
# ripgrep alone
$ time rg execve >/dev/null
rg execve > /dev/null  1.02s user 0.85s system 1075% cpu 0.174 total

# git-grep alone
$ time git grep execve >/dev/null
git grep execve > /dev/null  1.16s user 2.00s system 476% cpu 0.664 total

# ripgrep as a prefilter
$ time (rg -l execve | xargs git grep execve) >/dev/null
( rg -l execve | xargs git grep execve; ) > /dev/null  1.15s user 0.92s system 742% cpu 0.280 total

# ripgrep as a prefilter, with --show-function
$ time (rg -l execve | xargs git grep --show-function execve) >/dev/null
( rg -l execve | xargs git grep --show-function execve; ) > /dev/null  1.14s user 0.91s system 748% cpu 0.275 total
```

From my perspective, it is a very useful feature which has a demonstrably low overhead in existing implementations.  It's a bit at odds with "general purpose line grep" (as you've stated before in #236 and #282 for similar requests), but I think it has utility.

Is this something `ripgrep` would entertain as a feature?

---

_Comment by @zachriggle on 2017-11-09 21:24_

This appears to be a dupe of #502 which I didn't see before writing this, so I'll leave it here.

---

_Comment by @BurntSushi on 2017-11-09 21:29_

@zachriggle Thanks for the detailed write up! And Yeah, this is definitely a dupe of #502, so I'm going to close this one. I also added labels to #502 to indicate how I currently view it, which is basically, "I still have a lot of questions about the maintenance burden this imposes, and even if I'd be OK with maintaining such a feature, it isn't something I personally plan to do for a while."

---

_Closed by @BurntSushi on 2017-11-09 21:29_

---

_Label `duplicate` added by @BurntSushi on 2017-11-09 21:29_

---

_Comment by @zachriggle on 2017-11-09 21:44_

Actually, on follow-up I realized that I had forgotten that modern `git grep` *DOES* support `--break --heading --line-number` and so on, replicating the output that I want.

Ultimately, my work-around for support for this is to have the following script named `git-rg` in my `$PATH`: https://github.com/zachriggle/config/blob/master/bin/git-rg

As you can see, this produces the output I desire with MUCH better performance than `git grep` and also has better performance than `rg` by itself.

```
git grep kasan_unpoison_remaining_stack  0.52s user 1.74s system 192% cpu 1.170 total
rg kasan_unpoison_remaining_stack  0.76s user 1.32s system 594% cpu 0.351 total
git rg kasan_unpoison_remaining_stack  0.66s user 1.31s system 612% cpu 0.322 total
```

The output looks like:

```
$ git rg kasan_unpoison_remaining_stack
arch/arm64/kernel/sleep.S
119=cpu_resume_after_mmu:
122:    bl      kasan_unpoison_remaining_stack
144=ENTRY(_cpu_resume)
168:    bl      kasan_unpoison_remaining_stack

mm/kasan/kasan.c
74=void kasan_unpoison_task_stack(struct task_struct *task)
80:asmlinkage void kasan_unpoison_remaining_stack(void *sp)
```

---

_Comment by @YeungOnion on 2022-07-25 23:09_

@zachriggle I can't access the repo or file you linked. Do you still have that script?

> Ultimately, my work-around for support for this is to have the following script named `git-rg` in my `$PATH`: https://github.com/zachriggle/config/blob/master/bin/git-rg

I'm wanting to know if you went with something like this
```
rg -l execve | xargs git grep --show-function --heading --line-number --break execve
```
or was it something quite distinct?


---

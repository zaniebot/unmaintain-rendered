```yaml
number: 3259
title: "`rg` cannot traverse arbitrarily deep directories like GNU `grep`."
type: issue
state: open
author: collinfunk
labels:
  - bug
assignees: []
created_at: 2026-01-07T05:37:41Z
updated_at: 2026-01-08T02:43:58Z
url: https://github.com/BurntSushi/ripgrep/issues/3259
synced_at: 2026-01-12T16:13:25Z
```

# `rg` cannot traverse arbitrarily deep directories like GNU `grep`.

---

_@collinfunk_

Here is how you can reproduce:

```
$ rg --version | head -n 1
ripgrep 15.1.0 (rev 0a88cccd51)
$ grep --version | head -n 1
grep (GNU grep) 3.12.14-071a
$ mkdir -p `yes a/ | head -n $((32 * 1024)) | tr -d '\n'`
$ while cd $(yes a/ | head -n 1024 | tr -d '\n'); do :; done 2>/dev/null
$ pwd | tr '/' '\n' | wc -l
32771
$ echo hello | tee $(seq 100) > /dev/null
$ (cd $HOME && grep -r '^hello$' a | wc -l)
100
$ (cd $HOME && rg '^hello$' a)
rg: [trimmed long file name]: File name too long (os error 36)
```

It also seems that `rg` unnecessarily computes the current working
directory for each file processed. That becomes inefficient when
`ENAMETOOLONG` is returned and the `openat (dirfd, "..", ...)` to root
fallback has to be used. This can be seen below from the same directory:

```
$ pwd | tr '/' '\n' | wc -l
32771
$ time grep -r '^hello$' | wc -l
100

real	0m0.004s
user	0m0.001s
sys	0m0.005s
$ time rg --no-heading --line-number '^hello$' | wc -l

thread '<unknown>' (1405793) has overflowed its stack
fatal runtime error: stack overflow, aborting
100

real	2m59.479s
user	0m22.209s
sys	19m10.741s
```

I am not sure where the stack overflow is from. Probably a large item on
the stack, instead of deep recursion?

Also, during that test you can see that lots of memory is allocated
seemingly only for the file names:

```
$ (ulimit -v $(($(numfmt --from=iec 9G) / 1024)) && rg --no-heading --line-number '^hello$')
memory allocation of 63663 bytes failed
Aborted                    (core dumped) ( ulimit -v $(($(numfmt --from=iec 9G) / 1024)) && rg --no-heading --line-number '^hello$' )
```

Note that creating a directory this deep might depend on your file
system, and shells other than `bash` or `zsh` will almost certainly fail
to `cd` this deep.


---

_Comment by @BurntSushi on 2026-01-07 13:35_

Thanks for the report!

This is unfortunately a difficult bug to fix because it requires somewhat invasive changes to `walkdir` (single threaded directory traversal) and `ignore` (multi-threaded directory traversal). See https://github.com/BurntSushi/walkdir/issues/181 and https://github.com/BurntSushi/walkdir/issues/23. I started a journey a few years to fix this, but was never able to finish it. It's pretty low priority for me.

> It also seems that rg unnecessarily computes the current working directory for each file processed.

If ripgrep is actually doing this, then that's a bug that should be fixed independent of the long file path issue. ripgrep should only be asking for the CWD once:

https://github.com/BurntSushi/ripgrep/blob/0a88cccd5188074de96f54a4b6b44a63971ac157/crates/core/flags/hiargs.rs#L1292-L1312

The CWD is used for matching gitignore patterns in some cases. (Something that GNU grep does not do.) It should probably be the case that if ripgrep doesn't _need_ the CWD, then it shouldn't ask for it. So, e.g., you should be able to run `rg --no-ignore` and have it not ask for the CWD. But ripgrep isn't smart enough for that today.

---

_Label `bug` added by @BurntSushi on 2026-01-07 13:35_

---

_Comment by @collinfunk on 2026-01-08 02:31_

> Thanks for the report!
> 
> This is unfortunately a difficult bug to fix because it requires somewhat invasive changes to `walkdir` (single threaded directory traversal) and `ignore` (multi-threaded directory traversal). See [BurntSushi/walkdir#181](https://github.com/BurntSushi/walkdir/issues/181) and [BurntSushi/walkdir#23](https://github.com/BurntSushi/walkdir/issues/23). I started a journey a few years to fix this, but was never able to finish it. It's pretty low priority for me.

I agree with your analysis in those bug reports. You certainly do not
want to use `chdir`, especially when the library is used by
multi-threaded programs. The correct thing to do is `openat` file
descriptors, and use the other `*at` functions where needed. My
understanding, which is very possibly outdated, is that Rust does not
have a stabilized interface for those. So it would require you using
`libc` and dealing with them directly.

For context, I help maintain GNU coreutils. There we use the `fts` APIs
which originated in 4.4BSD for `chmod`, `chown`, `du`, etc. [1]. The
tiny catch there is that the version from 4.4BSD and in glibc use
`chdir` and/or `fchdir`. GNU coreutils, findutils, and grep import a
version from Gnulib which implements the additional option to use
`openat` and friends. Obviously, none of those programs use multiple
threads, so you have a bit more to consider.

> > It also seems that rg unnecessarily computes the current working directory for each file processed.
> 
> If ripgrep is actually doing this, then that's a bug that should be fixed independent of the long file path issue. ripgrep should only be asking for the CWD once:
>
> [...]
>
> The CWD is used for matching gitignore patterns in some cases. (Something that GNU grep does not do.) It should probably be the case that if ripgrep doesn't _need_ the CWD, then it shouldn't ask for it. So, e.g., you should be able to run `rg --no-ignore` and have it not ask for the CWD. But ripgrep isn't smart enough for that today.

Makes sense, certainly a separate issue and a very uncommon one. I had a
patch for GNU grep to check version control ignores, mostly copied from
`tar --exclude-vcs-ignores`, but it wasn't fully completed. I think I
used `open` with the relative path, but getting the current working
directory once is fine.

[1] https://man7.org/linux/man-pages/man3/fts_open.3.html

---

_Comment by @BurntSushi on 2026-01-08 02:43_

Yeah there is a branch on the walkdir repository with an in progress refactor to use openat. And yes, indeed, it has to use libc directly.

---

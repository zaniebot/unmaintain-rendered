```yaml
number: 2565
title: "perf(ignore): don't search subdirs for git/ignore files if max depth is reached"
type: issue
state: open
author: sigmaSd
labels:
  - help wanted
assignees: []
created_at: 2023-07-23T08:21:21Z
updated_at: 2023-08-02T13:01:46Z
url: https://github.com/BurntSushi/ripgrep/issues/2565
synced_at: 2026-01-12T16:13:24Z
```

# perf(ignore): don't search subdirs for git/ignore files if max depth is reached

---

_@sigmaSd_

When searching for git/ignore files (the one set with `.ignore` `.git_ignore` `.git_exclude`), the crate should not search the sub-directories if its at the max depth

example:
```
mkdir a
for ((i=1; i<=100; i++)); do     mktemp --directory XXX; done
```
a.rs
```rs
use ignore::WalkBuilder;

fn main() {
    let arg = std::env::args().nth(1).unwrap();
    let files: Vec<_> = WalkBuilder::new(&arg)
        .hidden(false)
        .follow_links(false) // We're scanning over depth 1
        .max_depth(Some(1))
        .build()
        .collect();
}
```
```
cargo b --release
strace target/release/a a
```

you can see that we do a lot of unneeded syscalls because we're reading git/ignore files even though we won't search inside those directorates
```
openat(AT_FDCWD, "/home/mrcool/dev/rust/others/ripgrep/crates/ignore/a/Vwe", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 4
newfstatat(4, "", {st_mode=S_IFDIR|0700, st_size=4096, ...}, AT_EMPTY_PATH) = 0
statx(AT_FDCWD, "/home/mrcool/dev/rust/others/ripgrep/crates/ignore/a/Vwe/.git", AT_STATX_SYNC_AS_STAT, STATX_ALL, 0x7fffdb02f030) = -1 ENOENT (Aucun fichier ou dossier de ce type)
statx(AT_FDCWD, "/home/mrcool/dev/rust/others/ripgrep/crates/ignore/a/Vwe/.ignore", AT_STATX_SYNC_AS_STAT, STATX_ALL, 0x7fffdb02ed60) = -1 ENOENT (Aucun fichier ou dossier de ce type)
statx(AT_FDCWD, "/home/mrcool/dev/rust/others/ripgrep/crates/ignore/a/Vwe/.gitignore", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|STATX_MNT_ID, stx_attributes=0, stx_mode=S_IFREG|0644, stx_size=0, ...}) = 0
openat(AT_FDCWD, "/home/mrcool/dev/rust/others/ripgrep/crates/ignore/a/Vwe/.gitignore", O_RDONLY|O_CLOEXEC) = 5
close(5)                                = 0
statx(AT_FDCWD, "/home/mrcool/dev/rust/others/ripgrep/crates/ignore/a/Vwe/.git/info/exclude", AT_STATX_SYNC_AS_STAT, STATX_ALL, 0x7fffdb02ed60) = -1 ENOENT (Aucun fichier ou dossier de ce type)
close(4)                                = 0
```
this is the best case (empty dirs), in practice its worse because directory can have those files, and this crate will have to issue read syscalls for them

without that search, it would be only 3 syscalls per directory

**suggestion:**
I'm suggestion to not search for those files inside subdirs if we reach the max depth
Unless I'm missing something it should keep the exact same functionality while having all the performance boost

**motivation:**
you can checkout https://github.com/helix-editor/helix/issues/7715 where disabling this behavior improved the walking from ~4000 syscalls to ~900 and from 20 seconds to 100 ms in my old pc with hdd drive
so ~3000 wasted syscalls for ~300 files

---

_Comment by @sigmaSd on 2023-07-23 09:02_

To be clear this optimization only affects users which set `max_depth`

---

_Comment by @BurntSushi on 2023-07-23 12:02_

The implementation should already do this:

https://github.com/BurntSushi/ripgrep/blob/304a60e8e9d4b2a42dc3dfb1ba4cef6d7bf92515/crates/ignore/src/walk.rs#L1493-L1495

If it's not, I'm not sure why. I personally won't have a chance to investigate this probably until I give `ignore` a refresh. And I don't know when that will happen. Help is wanted if there's a simple fix.

---

_Label `help wanted` added by @BurntSushi on 2023-07-23 12:02_

---

_Comment by @sigmaSd on 2023-07-23 12:37_

That path seems to be only reached for `WalkParallel` but not `Walk` iterator

---

_Comment by @BurntSushi on 2023-07-23 12:38_

Yes, that's because the single threaded case uses [walkdir's `max_depth` option](https://docs.rs/walkdir/latest/walkdir/struct.WalkDir.html#method.max_depth).

---

_Comment by @sigmaSd on 2023-07-23 12:45_

The path that my example code take is this https://github.com/BurntSushi/ripgrep/blob/304a60e8e9d4b2a42dc3dfb1ba4cef6d7bf92515/crates/ignore/src/walk.rs#L1022 where add_child will internally do the expensive operations

---

_Comment by @BurntSushi on 2023-07-23 12:47_

Like I said, I haven't looked into this. I only know that `max_depth` is being used in some capacity. If there's an easy fix, patches are welcome.

---

_Comment by @AgustinRamiroDiaz on 2023-08-01 21:34_

@sigmaSd I've tried reproducing your steps but didn't hit the issue :thinking: 

Here's my last output while following your guide
```
execve("target/release/rg", ["target/release/rg"], 0x7ffe7abc8160 /* 71 vars */) = 0
brk(NULL)                               = 0x562e07ec1000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fffbb302e30) = -1 EINVAL (Invalid argument)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f82514dd000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=80371, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 80371, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f82514c9000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=125488, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 127720, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f82514a9000
mmap(0x7f82514ac000, 94208, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7f82514ac000
mmap(0x7f82514c3000, 16384, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1a000) = 0x7f82514c3000
mmap(0x7f82514c7000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d000) = 0x7f82514c7000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\237\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0 \0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0"..., 48, 848) = 48
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0i8\235HZ\227\223\333\350s\360\352,\223\340."..., 68, 896) = 68
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=2216304, ...}, AT_EMPTY_PATH) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 2260560, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f8251281000
mmap(0x7f82512a9000, 1658880, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x28000) = 0x7f82512a9000
mmap(0x7f825143e000, 360448, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1bd000) = 0x7f825143e000
mmap(0x7f8251496000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x214000) = 0x7f8251496000
mmap(0x7f825149c000, 52816, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f825149c000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f825127e000
arch_prctl(ARCH_SET_FS, 0x7f825127e800) = 0
set_tid_address(0x7f825127ead0)         = 61339
set_robust_list(0x7f825127eae0, 24)     = 0
rseq(0x7f825127f1a0, 0x20, 0, 0x53053053) = 0
mprotect(0x7f8251496000, 16384, PROT_READ) = 0
mprotect(0x7f82514c7000, 4096, PROT_READ) = 0
mprotect(0x562e069e0000, 192512, PROT_READ) = 0
mprotect(0x7f8251517000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7f82514c9000, 80371)           = 0
poll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f82512c3520}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x562e0692fa60, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7f82512c3520}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x562e0692fa60, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7f82512c3520}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f82514da000
mprotect(0x7f82514da000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0x7f82514db000, ss_flags=0, ss_size=8192}, NULL) = 0
getrandom("\x77\xba\xaa\x05\x8a\xeb\xbe\xfc", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x562e07ec1000
brk(0x562e07ee2000)                     = 0x562e07ee2000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
newfstatat(3, "", {st_mode=S_IFREG|0444, st_size=0, ...}, AT_EMPTY_PATH) = 0
read(3, "562e067ad000-562e067ea000 r--p 0"..., 1024) = 1024
read(3, "6000-7f825149a000 r--p 00214000 "..., 1024) = 1024
read(3, "linux-x86-64.so.2\n7f82514e1000-7"..., 1024) = 823
close(3)                                = 0
sched_getaffinity(61339, 32, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
write(2, "thread '", 8thread ')                 = 8
write(2, "main", 4main)                     = 4
write(2, "' panicked at '", 15' panicked at ')         = 15
write(2, "called `Option::unwrap()` on a `"..., 43called `Option::unwrap()` on a `None` value) = 43
write(2, "', ", 3', )                      = 3
write(2, "crates/core/main.rs", 19crates/core/main.rs)     = 19
write(2, ":", 1:)                        = 1
write(2, "51", 251)                       = 2
write(2, ":", 1:)                        = 1
write(2, "39", 239)                       = 2
write(2, "\n", 1
)                       = 1
write(2, "note: run with `RUST_BACKTRACE=1"..., 78note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
) = 78
futex(0x7f82514c8210, FUTEX_WAKE_PRIVATE, 2147483647) = 0
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=8192}, NULL) = 0
munmap(0x7f82514da000, 12288)           = 0
exit_group(101)                         = ?
+++ exited with 101 +++
``` 

---

_Comment by @sigmaSd on 2023-08-01 21:39_

@AgustinRamiroDiaz I'm not sure what steps are you following,  you seem to be spawning rg for some reason 

---

_Comment by @AgustinRamiroDiaz on 2023-08-02 12:08_

@sigmaSd yeah, I forgot to clarify

I couldn't follow this steps
```
cargo b --release
strace target/release/a a
```

because I wasn't getting a `target/release/a` binary. So instead of creating the `a.rs` file I've modified the `main.rs` to be
use ignore::WalkBuilder;

```
fn main() {
    let arg = std::env::args().nth(1).unwrap();
    let files: Vec<_> = WalkBuilder::new(&arg)
        .hidden(false)
        .follow_links(false) // We're scanning over depth 1
        .max_depth(Some(1))
        .build()
        .collect();
}
```

and then ran
```
cargo b --release
strace target/release/rg
```

---

_Comment by @sigmaSd on 2023-08-02 13:01_

As shown above you have to create a directory with nested subdirectories, then you can strace the compiled program against the root dir

---

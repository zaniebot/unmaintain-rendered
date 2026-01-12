```yaml
number: 916
title: Zombie processes cause rg --files to hang in /proc
type: issue
state: closed
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2018-05-10T10:14:10Z
updated_at: 2019-01-27T18:15:53Z
url: https://github.com/BurntSushi/ripgrep/issues/916
synced_at: 2026-01-12T16:13:22Z
```

# Zombie processes cause rg --files to hang in /proc

---

_@sharkdp_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1 (rev ab64da73ab)
-SIMD -AVX
```

#### What operating system are you using ripgrep on?

```
> uname -s -r -v -m -p -i -o
Linux 4.16.7-1-ARCH #1 SMP PREEMPT Wed May 2 21:12:36 UTC 2018 x86_64 unknown unknown GNU/Linux
```

#### Describe the bug.

Even if it does not seem like a good idea in general to search the whole file system without excluding special folders like `/proc` (see also #311: *"TL;DR is: don't search /proc"*), I think that users will attempt to do so.

We recently tracked down one specific reproducible state which causes the `ignore` crate to hang while traversing `/proc` (see https://github.com/sharkdp/fd/issues/288 for more details). The same bug can be reproduced with `rg --files`.

#### If this is a bug, what are the steps to reproduce the behavior?

1. Make sure that there are no zombie processes on your system (`ps -ef | rg '<defunct>'`)

2. Run `rg --files > /dev/null 2>&1` inside `/proc`. Everything should work fine (process exits after a few seconds).

3. Create a zombie process on purpose
    a. Copy the code from https://stackoverflow.com/a/25228579/704831 into a file called `zombie.c`
    b. Compile it `gcc -o zombie zombie.c`
    c. Run it: `./zombie`

4. Run `rg --files > /dev/null 2>&1` inside `/proc` again. This seems to hang indefinitely.

5. Get the PID of the zombie process via `ps -ef | rg '<defunct>'`. Call it `$PID`.

6. Run `rg --glob "\!$PID" --files > /dev/null 2>&1`. This should work fine, indicating that the directory traversal hangs in `/proc/$PID`.

#### If this is a bug, what is the actual behavior?

`rg --files` hangs indefinitely inside the `/proc` folder.

#### If this is a bug, what is the expected behavior?

`rg --files` traverses the `/proc` folder in finite time.

Note: `find`, for example, does not seem to have this problem and finishes in finite time even in the presence of zombie processes.

---

_Comment by @BurntSushi on 2018-05-10 11:33_

Fascinating bug! `strace` reveals some of the secrets here:

```
execve("/home/andrew/rust/ripgrep/target/release/rg", ["rg", "--files", "--no-messages", "-j1", "/proc/29082"], 0x7ffc2f21e700 /* 87 vars */) = 0
brk(NULL)                               = 0x55625317e000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=213799, ...}) = 0
mmap(NULL, 213799, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f3040e98000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@\r\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=14160, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f3040e96000
mmap(NULL, 2109584, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f3040aa5000
mprotect(0x7f3040aa8000, 2093056, PROT_NONE) = 0
mmap(0x7f3040ca7000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f3040ca7000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/librt.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0000\36\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=30920, ...}) = 0
mmap(NULL, 2128376, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f304089d000
mprotect(0x7f30408a4000, 2093056, PROT_NONE) = 0
mmap(0x7f3040aa3000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0x7f3040aa3000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0]\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=145336, ...}) = 0
mmap(NULL, 2216400, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f304067f000
mprotect(0x7f3040698000, 2093056, PROT_NONE) = 0
mmap(0x7f3040897000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x18000) = 0x7f3040897000
mmap(0x7f3040899000, 12752, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f3040899000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0(\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=747480, ...}) = 0
mmap(NULL, 2188016, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f3040468000
mprotect(0x7f304047e000, 2093056, PROT_NONE) = 0
mmap(0x7f304067d000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15000) = 0x7f304067d000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`\20\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=2065784, ...}) = 0
mmap(NULL, 3893488, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f30400b1000
mprotect(0x7f304025f000, 2093056, PROT_NONE) = 0
mmap(0x7f304045e000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1ad000) = 0x7f304045e000
mmap(0x7f3040464000, 14576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f3040464000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f3040e94000
arch_prctl(ARCH_SET_FS, 0x7f3040e95380) = 0
mprotect(0x7f304045e000, 16384, PROT_READ) = 0
mprotect(0x7f304067d000, 4096, PROT_READ) = 0
mprotect(0x7f3040897000, 4096, PROT_READ) = 0
mprotect(0x7f3040aa3000, 4096, PROT_READ) = 0
mprotect(0x7f3040ca7000, 4096, PROT_READ) = 0
mprotect(0x55625283a000, 176128, PROT_READ) = 0
mprotect(0x7f3040ecd000, 4096, PROT_READ) = 0
munmap(0x7f3040e98000, 213799)          = 0
set_tid_address(0x7f3040e95650)         = 29186
set_robust_list(0x7f3040e95660, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f3040684770, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f3040690dd0}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f3040684810, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f3040690dd0}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
readlink("/etc/malloc.conf", 0x7ffc91c9a220, 4096) = -1 ENOENT (No such file or directory)
open("/proc/sys/vm/overcommit_memory", O_RDONLY) = 3
read(3, "0", 1)                         = 1
close(3)                                = 0
brk(NULL)                               = 0x55625317e000
mmap(NULL, 2097152, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_NORESERVE, -1, 0) = 0x7f303feb1000
munmap(0x7f303feb1000, 2097152)         = 0
mmap(NULL, 4190208, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_NORESERVE, -1, 0) = 0x7f303fcb2000
munmap(0x7f303fcb2000, 1368064)         = 0
munmap(0x7f3040000000, 724992)          = 0
open("/sys/devices/system/cpu/online", O_RDONLY|O_CLOEXEC) = 3
read(3, "0-3\n", 8192)                  = 4
close(3)                                = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f30400e58e0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
mmap(NULL, 2097152, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_NORESERVE, -1, 0) = 0x7f303fc00000
open("/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
fstat(3, {st_mode=S_IFREG|0444, st_size=0, ...}) = 0
read(3, "5562522d5000-55625263a000 r-xp 0"..., 1024) = 1024
read(3, "06                     /usr/lib/"..., 1024) = 1024
read(3, " 788474                     /usr"..., 1024) = 1024
close(3)                                = 0
sched_getaffinity(29186, 32, [0, 1, 2, 3]) = 32
rt_sigaction(SIGSEGV, {sa_handler=0x5562524f34b0, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7f3040690dd0}, NULL, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x5562524f34b0, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7f3040690dd0}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f3040ecb000
sigaltstack({ss_sp=0x7f3040ecb000, ss_flags=0, ss_size=8192}, NULL) = 0
rt_sigaction(SIGPIPE, {sa_handler=SIG_DFL, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f30400e58e0}, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7f30400e58e0}, 8) = 0
getrandom("", 0, GRND_NONBLOCK)         = 0
getrandom("\xe7\x0e\x06\xc4\x60\xfe\xff\xfc\x06\x8a\x55\x44\x2c\xcb\xeb\xbb", 16, GRND_NONBLOCK) = 16
openat(AT_FDCWD, "/home/andrew/.ripgreprc", O_RDONLY|O_CLOEXEC) = 3
ioctl(3, FIOCLEX)                       = 0
read(3, "--max-columns=200\n--colors=match"..., 8192) = 167
read(3, "", 8192)                       = 0
close(3)                                = 0
ioctl(1, TCGETS, 0x7ffc91c99d10)        = -1 ENOTTY (Inappropriate ioctl for device)
stat("/proc/29082", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
stat("/proc/29082", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
ioctl(1, TCGETS, 0x7ffc91c99d10)        = -1 ENOTTY (Inappropriate ioctl for device)
getcwd("/proc", 512)                    = 6
ioctl(1, TCGETS, 0x7ffc91c99d10)        = -1 ENOTTY (Inappropriate ioctl for device)
fstat(1, {st_mode=S_IFREG|0664, st_size=7514, ...}) = 0
fstat(1, {st_mode=S_IFREG|0664, st_size=7570, ...}) = 0
stat("/proc/29082", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
openat(AT_FDCWD, "/home/andrew/.gitconfig", O_RDONLY|O_CLOEXEC) = 3
ioctl(3, FIOCLEX)                       = 0
read(3, "[push]\n\tdefault = simple\n[rerere"..., 8192) = 224
read(3, "", 8192)                       = 0
close(3)                                = 0
stat("/home/andrew/.config/git/ignore", 0x7ffc91c993c0) = -1 ENOENT (No such file or directory)
stat("/proc/29082", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
lstat("/proc", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
lstat("/proc/29082", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
openat(AT_FDCWD, "/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/.git", 0x7ffc91c990d0)           = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/proc/.git", 0x7ffc91c990d0)      = -1 ENOENT (No such file or directory)
stat("/proc/29082", {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
open("/proc/29082", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
fstat(3, {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
openat(AT_FDCWD, "/proc/29082/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/proc/29082/.git", 0x7ffc91c99670) = -1 ENOENT (No such file or directory)
getdents(3, /* 51 entries */, 32768)    = 1528
open("/proc/29082/task", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 4
fstat(4, {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
openat(AT_FDCWD, "/proc/29082/task/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/proc/29082/task/.git", 0x7ffc91c99670) = -1 ENOENT (No such file or directory)
getdents(4, /* 3 entries */, 32768)     = 80
open("/proc/29082/task/29082", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 5
fstat(5, {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
openat(AT_FDCWD, "/proc/29082/task/29082/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/proc/29082/task/29082/.git", 0x7ffc91c99670) = -1 ENOENT (No such file or directory)
getdents(5, /* 45 entries */, 32768)    = 1328
open("/proc/29082/task/29082/fd", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fd/.rgignore", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fd/.ignore", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fd/.gitignore", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fd/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
stat("/proc/29082/task/29082/fd/.git", 0x7ffc91c99670) = -1 EACCES (Permission denied)
open("/proc/29082/task/29082/fdinfo", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fdinfo/.rgignore", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fdinfo/.ignore", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fdinfo/.gitignore", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/fdinfo/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
stat("/proc/29082/task/29082/fdinfo/.git", 0x7ffc91c99670) = -1 EACCES (Permission denied)
open("/proc/29082/task/29082/ns", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 EACCES (Permission denied)
openat(AT_FDCWD, "/proc/29082/task/29082/ns/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/ns/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/ns/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/ns/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/proc/29082/task/29082/ns/.git", 0x7ffc91c99670) = -1 ENOENT (No such file or directory)
open("/proc/29082/task/29082/net", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 6
fstat(6, {st_mode=S_IFDIR|0555, st_size=0, ...}) = 0
openat(AT_FDCWD, "/proc/29082/task/29082/net/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/net/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/net/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/29082/task/29082/net/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
stat("/proc/29082/task/29082/net/.git", 0x7ffc91c99670) = -1 ENOENT (No such file or directory)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)
...
```

In particular, `getdents(6, 0x7f303fd9f5f0, 32768)      = -1 EINVAL (Invalid argument)` is repeated in what appears to be an infinite loop. This happens even when ripgrep uses walkdir directly (forced by passing `-j1`). If I run walkdir's example program on the directory, then I see this output:

```
/proc/29271
/proc/29271/task
/proc/29271/task/29271
/proc/29271/task/29271/fd
ERROR: IO error for operation on /proc/29271/task/29271/fd: Permission denied (os error 13)
/proc/29271/task/29271/fdinfo
ERROR: IO error for operation on /proc/29271/task/29271/fdinfo: Permission denied (os error 13)
/proc/29271/task/29271/ns
ERROR: IO error for operation on /proc/29271/task/29271/ns: Permission denied (os error 13)
/proc/29271/task/29271/net
ERROR: Invalid argument (os error 22)
ERROR: Invalid argument (os error 22)
ERROR: Invalid argument (os error 22)
ERROR: Invalid argument (os error 22)
...
```

with the invalid argument error appearing to be coming from a loop.

If I look at the strace logs in `find`, I see the same `EINVAL` errors, but no looping.

Reading the man page for `getdents`, it says that `EINVAL` means "Result buffer is too small."

Following the strace logs, the problematic directory appears to be `/proc/31439/task/31439/net`:

```
$ ls /proc/31439/task/31439/net
ls: reading directory '/proc/31439/task/31439/net': Invalid argument
```

I've scoured the man pages of `readdir(2)` (Linux, no longer relevant?), `readdir(3)` (POSIX) and `getdents` (the last being the raw syscall), and I can't find *anything* that specifies the behavior of these routines when an error is returned. In particular, the fact that `readdir` can return `EINVAL` isn't apparently specified by POSIX. My guess at this particular moment in time is that the directory stream opened by `opendir` is not advanced either when this particular error occurs or when any error occurs. This explains the infinite loop, since errors that occur during directory traversal don't stop traversal.

Given the lack of specification, the next best thing is to go out and look at how tools like `find` (or probably `fts`) handle this case. When `readdir` returns an error, do they give up on reading that directory immediately? Is there case analysis on the type of error where some errors result in giving up but others result in mushing on? I don't know.

---

_Comment by @BurntSushi on 2018-05-10 11:34_

Thanks for the excellent reproducible bug report!!

---

_Label `bug` added by @BurntSushi on 2018-05-10 11:35_

---

_Comment by @sharkdp on 2018-05-10 15:21_

Thank you for your detailed response.


I can actually reproduce this by using just `std::fs::read_dir`:

``` rust
use std::env;
use std::io;
use std::fs;
use std::path::Path;

fn visit_dirs(dir: &Path) -> io::Result<()> {
    for entry in fs::read_dir(dir)? {
        println!("{:?}", entry);

        if let Ok(entry) = entry {
            let path = entry.path();
            if path.is_dir() {
                let _ = visit_dirs(&path);
            }
        }
    }
    Ok(())
}

fn main() {
    let root = env::args().nth(1).unwrap();
    let _ = visit_dirs(Path::new(&root));
}
```

Which hangs in the same loop:
```
> cargo run -- /proc/4032
...
Err(Os { code: 22, kind: InvalidInput, message: "Invalid argument" })
Err(Os { code: 22, kind: InvalidInput, message: "Invalid argument" })
Err(Os { code: 22, kind: InvalidInput, message: "Invalid argument" })
...
```

---

_Comment by @BurntSushi on 2018-05-10 15:23_

Ah yeah thanks for doing that. That was going to be my next test, but ran out of time.

Probably the next step is to scrutinize `find`/`fts` and see how they handle the errors returned by `readdir`. AIUI, `fs::read_dir` is basically just a pretty thin layer on top of a loop that calls `readdir`.

---

_Comment by @sharkdp on 2018-05-10 15:24_

Oh, I just found this: https://github.com/BurntSushi/walkdir/issues/98

---

_Comment by @BurntSushi on 2018-05-10 15:26_

Nice! Forgot about that. :-)

---

_Comment by @sharkdp on 2018-05-10 17:37_

So I did some more debugging and I believe I found the cause of this.

When called on `/proc/$ZOMBIE_PID/net`, the `readdir_r` function

``` c
int readdir_r(DIR *dirp, struct dirent *entry, struct dirent **result);
```

returns error code `22` (as we have seen above) and, **at the same time**, returns `NULL` in `*result`, signalling the end of the directory stream. If I am reading the code in the standard library correctly, this case can not be handled properly at the moment.

``` rust
loop {
    if readdir64_r(self.dirp.0, &mut ret.entry, &mut entry_ptr) != 0 {
        return Some(Err(Error::last_os_error()))
    }
    if entry_ptr.is_null() {
        return None
    }
    if ret.name_bytes() != b"." && ret.name_bytes() != b".." {
        return Some(Ok(ret))
    }
}
```
(Code from the `next` function of `impl Iterator for ReadDir`, see https://github.com/rust-lang/rust/blob/f25c2283b3f8a7518b2f83a252b50d29d9bfbfda/src/libstd/sys/unix/fs.rs#L252-L262)

To handle this properly (without looping forever), one would probably have to check for `entry_ptr.is_null()` in the first (`Some(Err(...))`) case as well and store the result in some internal state of the iterator. On the next call of `next`, it would have to return `None`.

What do you think? Is this something that should be reported upstream?

---

_Comment by @BurntSushi on 2018-05-10 17:44_

@sharkdp Nice find! I think I do indeed agree with your conclusion that this should be fixed upstream.

---

_Comment by @sharkdp on 2018-05-10 18:27_

Reported here: https://github.com/rust-lang/rust/issues/50619

---

_Comment by @BurntSushi on 2018-08-22 00:12_

This should be fixed in the next stable release (1.29.0). I've confirmed that it is fixed in the current beta of 1.29.0, but is still present in Rust 1.28.0.

---

_Closed by @BurntSushi on 2019-01-27 18:15_

---

```yaml
number: 331
title: Core dump with 0.4.0 Linux x86_64 release binary
type: issue
state: closed
author: moorereason
labels: []
assignees: []
created_at: 2017-01-17T23:06:48Z
updated_at: 2018-03-13T03:22:17Z
url: https://github.com/BurntSushi/ripgrep/issues/331
synced_at: 2026-01-12T16:13:21Z
```

# Core dump with 0.4.0 Linux x86_64 release binary

---

_@moorereason_

Using the `ripgrep-0.4.0-x86_64-unknown-linux-musl` release binary, I receive a core dump in a Arch Linux Virtual Box VM (v5.1.14r112924).  Tried on a physical (Intel Xeon) CentOS 7 server, and it works fine there.  I don't have a Rust dev env setup, but I probably could if I need to.  Let me know if you need more details, but here's what I'm seeing:

```
$ rg
error: The following required arguments were not provided:
    <pattern>

USAGE:

    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try --help
```

```
$ rg ls
Illegal instruction (core dumped)
```

```
$ gdb ~/bin/rg
GNU gdb (GDB) 7.12
This GDB was configured as "x86_64-pc-linux-gnu".
Reading symbols from /home/USER/bin/rg...done.
warning: Missing auto-load script at offset 0 in section .debug_gdb_scripts
of file /home/USER/bin/rg.
Use `info auto-load python-scripts [REGEXP]' to list them.
(gdb) run test
Starting program: /home/USER/bin/rg test
[New LWP 19001]
[New LWP 19002]

Thread 3 "rg" received signal SIGILL, Illegal instruction.
[Switching to LWP 19002]
0x00000000004dbd70 in simd::u8x16::splat (x=<optimized out>) at /home/travis/.cargo/registry/src/github.com-1ecc6299db9ec823/simd-0.1.1/src/common.rs:152
152     /home/travis/.cargo/registry/src/github.com-1ecc6299db9ec823/simd-0.1.1/src/common.rs: No such file or directory.
```

```
$ strace ~/bin/rg live
execve("/home/USER/bin/rg", ["/home/USER/bin/rg", "live"], [/* 33 vars */]) = 0
mmap(NULL, 648, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea896a000
arch_prctl(ARCH_SET_FS, 0x7fbea896a138) = 0
set_tid_address(0x7fbea896a170)         = 18268
readlink("/etc/malloc.conf", 0x7ffea2239c40, 4096) = -1 ENOENT (No such file or directory)
brk(NULL)                               = 0x1fad000
mmap(NULL, 2097152, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea876a000
munmap(0x7fbea876a000, 2097152)         = 0
mmap(NULL, 4190208, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea856b000
munmap(0x7fbea856b000, 610304)          = 0
munmap(0x7fbea8800000, 1482752)         = 0
sched_getaffinity(0, 128, [0, 1])       = 16
mmap(NULL, 2097152, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea8400000
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x57d4b3}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RT_1 RT_2], NULL, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x547ac0, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_SIGINFO, sa_restorer=0x57d4b3}, NULL, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x547ac0, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_SIGINFO, sa_restorer=0x57d4b3}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea8968000
sigaltstack({ss_sp=0x7fbea8968000, ss_flags=0, ss_size=8192}, NULL) = 0
getrandom("", 0, GRND_NONBLOCK)         = 0
getrandom("q\2652\217\\\n\276(", 8, GRND_NONBLOCK) = 8
getrandom("\345\27\337\365\272U\261U", 8, GRND_NONBLOCK) = 8
ioctl(0, TIOCGWINSZ, {ws_row=64, ws_col=237, ws_xpixel=0, ws_ypixel=0}) = 0
stat("./", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
stat("./", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
ioctl(1, TIOCGWINSZ, {ws_row=64, ws_col=237, ws_xpixel=0, ws_ypixel=0}) = 0
ioctl(1, TIOCGWINSZ, {ws_row=64, ws_col=237, ws_xpixel=0, ws_ypixel=0}) = 0
getcwd("/home/USER/src/github.com/spf13/hugo", 512) = 39
ioctl(1, TIOCGWINSZ, {ws_row=64, ws_col=237, ws_xpixel=0, ws_ypixel=0}) = 0
ioctl(1, TIOCGWINSZ, {ws_row=64, ws_col=237, ws_xpixel=0, ws_ypixel=0}) = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 3), ...}) = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 3), ...}) = 0
sched_getaffinity(0, 128, [0, 1])       = 16
stat("./", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
open("/home/USER/.gitconfig", O_RDONLY|O_CLOEXEC) = 3
fcntl(3, F_SETFD, FD_CLOEXEC)           = 0
ioctl(3, FIOCLEX)                       = 0
read(3, "# This is Git's per-user configu"..., 8192) = 178
read(3, "", 8192)                       = 0
close(3)                                = 0
stat("/home/USER/git/ignore", 0x7ffea2234ea0) = -1 ENOENT (No such file or directory)
stat("./", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
rt_sigprocmask(SIG_UNBLOCK, [RT_1 RT_2], NULL, 8) = 0
mmap(NULL, 2105344, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea81fe000
mprotect(0x7fbea81ff000, 2101248, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbea83ff968, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID|0x400000, parent_tidptr=0x7fbea83ffae8, tls=0x7fbea83ffab0, child_tidptr
=0x7fbea83ffae8) = 18269
mmap(NULL, 2105344, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbea7ffc000
mprotect(0x7fbea7ffd000, 2101248, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbea81fd968, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID|0x400000, parent_tidptr=0x7fbea81fdae8, tls=0x7fbea81fdab0, child_tidptr
=0x7fbea81fdae8) = 18270
futex(0x7fbea83ffae8, FUTEX_WAIT, 18269, NULL) = ?
+++ killed by SIGILL (core dumped) +++
Illegal instruction (core dumped)
```

```
$ uname -a
Linux localhost 4.8.13-1-ARCH #1 SMP PREEMPT Fri Dec 9 07:24:34 CET 2016 x86_64 GNU/Linux
```

One of two processors:
```
$ cat /proc/cpuinfo
processor       : 0
vendor_id       : AuthenticAMD
cpu family      : 16
model           : 4
model name      : AMD Phenom(tm) II X2 B55 Processor
stepping        : 3
microcode       : 0x1000086
cpu MHz         : 3000.206
cache size      : 512 KB
physical id     : 0
siblings        : 2
core id         : 0
cpu cores       : 2
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 5
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt rdtscp lm 3dnowext 3dnow rep_good nopl extd_apicid eagerfpu pni cx16 popcnt hypervisor lahf_lm cmp_legacy cr8_legacy abm sse4a misalignsse 3dnowprefetch vmmcall
bugs            : tlb_mmatch apic_c1e fxsave_leak sysret_ss_attrs null_seg
bogomips        : 6002.42
TLB size        : 1024 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management:
```


---

_Comment by @BurntSushi on 2017-01-17 23:09_

Dupe of #135.

I'm afraid you'll need to compile from source or install ripgrep from the community repo. Alternatively, figure out how to get ssse3 support in your VM.

---

_Closed by @BurntSushi on 2017-01-17 23:09_

---

_Comment by @BurntSushi on 2018-03-13 03:22_

This should be fixed by https://github.com/BurntSushi/ripgrep/pull/857 in the next release.

---

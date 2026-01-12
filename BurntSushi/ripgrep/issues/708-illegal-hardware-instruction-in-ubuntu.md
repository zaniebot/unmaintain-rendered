```yaml
number: 708
title: illegal hardware instruction in ubuntu
type: issue
state: closed
author: hpcsc
labels:
  - duplicate
assignees: []
created_at: 2017-12-06T08:05:28Z
updated_at: 2018-03-13T03:22:14Z
url: https://github.com/BurntSushi/ripgrep/issues/708
synced_at: 2026-01-12T16:13:22Z
```

# illegal hardware instruction in ubuntu

---

_@hpcsc_

I have an Ubuntu VM on Azure (16.04.3 LTS (GNU/Linux 4.11.0-1015-azure x86_64)), when I run `rg`, I have following error:

```
[1]    13638 illegal hardware instruction (core dumped)  rg debian
```

Not sure where I go wrong. I installed it by downloading from github:

```
curl -L https://github.com/BurntSushi/ripgrep/releases/download/0.7.1/ripgrep-0.7.1-x86_64-unknown-linux-musl.tar.gz -o ripgrep.tar.gz
```

then extract and move `rg` executable to `/usr/local/bin`

---

_Comment by @BurntSushi on 2017-12-06 10:29_

Please show the output of `cat /proc/cpuinfo`.

On Dec 6, 2017 03:05, "David Nguyen" <notifications@github.com> wrote:

> I have an Ubuntu VM on Azure (16.04.3 LTS (GNU/Linux 4.11.0-1015-azure
> x86_64)), when I run rg, I have following error:
>
> [1]    13638 illegal hardware instruction (core dumped)  rg debian
>
> Not sure where I go wrong. I installed it by downloading from github:
>
> curl -L https://github.com/BurntSushi/ripgrep/releases/download/0.7.1/ripgrep-0.7.1-x86_64-unknown-linux-musl.tar.gz -o ripgrep.tar.gz
>
> then extract and move rg executable to /usr/local/bin
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/708>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oNWbCdikOSGit_CIRhJDbeLBYOfks5s9krJgaJpZM4Q3g-g>
> .
>


---

_Comment by @hpcsc on 2017-12-06 12:02_

Below is the output of that command:

```
processor	: 0
vendor_id	: AuthenticAMD
cpu family	: 16
model		: 8
model name	: AMD Opteron(tm) Processor 4171 HE
stepping	: 1
microcode	: 0xffffffff
cpu MHz		: 2094.722
cache size	: 512 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 5
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm 3dnowext 3dnow rep_good nopl cpuid extd_apicid pni cx16 popcnt hypervisor lahf_lm cmp_legacy cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw vmmcall
bugs		: tlb_mmatch fxsave_leak sysret_ss_attrs null_seg amd_e400
bogomips	: 4189.44
TLB size	: 1024 4K pages
clflush size	: 64
cache_alignment	: 64
address sizes	: 44 bits physical, 48 bits virtual
power management:
```

---

_Comment by @BurntSushi on 2017-12-06 12:14_

OK, this is a duplicate of #135. Your CPU doesn't support SSSE3, and the binary I distribute uses SSSE3 instructions. Right now, the only way to run ripgrep on such a machine is to compile it or install it from a distro specific package repository. (I believe the latter does not exist on Ubuntu/Debian.)

---

_Closed by @BurntSushi on 2017-12-06 12:14_

---

_Label `duplicate` added by @BurntSushi on 2017-12-06 12:14_

---

_Comment by @BurntSushi on 2018-03-13 03:22_

This should be fixed by https://github.com/BurntSushi/ripgrep/pull/857 in the next release.

---

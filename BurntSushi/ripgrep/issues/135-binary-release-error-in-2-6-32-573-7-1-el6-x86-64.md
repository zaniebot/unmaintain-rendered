```yaml
number: 135
title: Binary release error in 2.6.32-573.7.1.el6.x86_64 GNU/Linux
type: issue
state: closed
author: jeosadn
labels:
  - bug
  - question
assignees: []
created_at: 2016-09-29T20:54:00Z
updated_at: 2018-08-24T15:30:43Z
url: https://github.com/BurntSushi/ripgrep/issues/135
synced_at: 2026-01-12T16:13:21Z
```

# Binary release error in 2.6.32-573.7.1.el6.x86_64 GNU/Linux

---

_@jeosadn_

When trying to run the ripgrep-0.2.1-x86_64-unknown-linux-musl.tar.gz release the following error happens:

```
$ uname -or
2.6.32-573.7.1.el6.x86_64 GNU/Linux
$ ./rg
Illegal instruction (core dumped)
```

I tried to build it locally but ran into too many issues with cargo and rust (I'm behind the company firewall with a weird proxy and certificate handling)


---

_Comment by @BurntSushi on 2016-09-29 21:01_

Can you show the output of `/proc/cpuinfo`?

I build binaries with `ssse3` support. If your CPU doesn't support that, then I think this is the failure mode you'll see.

I can certainly put on binaries without SIMD support if that would help. (I was seeing if I could get away with not doing it, since it will be confusing to users. "Which one should I download?")


---

_Label `question` added by @BurntSushi on 2016-09-29 21:01_

---

_Comment by @jeosadn on 2016-09-29 21:15_

```
$ cat /proc/cpuinfo
processor       : 0
vendor_id       : AuthenticAMD
cpu family      : 16
model           : 9
model name      : AMD Opteron(tm) Processor 6136
stepping        : 1
cpu MHz         : 2400.000
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
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt rdtscp lm 3dnowext 3dnow constant_tsc rep_good tsc_reliable nonstop_tsc unfair_spinlock pni cx16 popcnt hypervisor lahf_lm extapic abm sse4a misalignsse 3dnowprefetch osvw
bogomips        : 4800.00
TLB size        : 1024 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 40 bits physical, 48 bits virtual
power management:

processor       : 1
vendor_id       : AuthenticAMD
cpu family      : 16
model           : 9
model name      : AMD Opteron(tm) Processor 6136
stepping        : 1
cpu MHz         : 2400.000
cache size      : 512 KB
physical id     : 0
siblings        : 2
core id         : 1
cpu cores       : 2
apicid          : 1
initial apicid  : 1
fpu             : yes
fpu_exception   : yes
cpuid level     : 5
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt rdtscp lm 3dnowext 3dnow constant_tsc rep_good tsc_reliable nonstop_tsc unfair_spinlock pni cx16 popcnt hypervisor lahf_lm extapic abm sse4a misalignsse 3dnowprefetch osvw
bogomips        : 4800.00
TLB size        : 1024 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 40 bits physical, 48 bits virtual
power management:

```


---

_Comment by @BurntSushi on 2016-09-29 22:40_

Yeah, looks like your CPU doesn't support `ssse3`, which probably explains the error. I'll work on generating sse2-only binaries, but for now, I'm afraid you'll have to compile from source if you want to use `ripgrep` on that machine.


---

_Label `bug` added by @BurntSushi on 2016-09-29 22:40_

---

_Label `question` removed by @BurntSushi on 2016-09-29 22:40_

---

_Comment by @BurntSushi on 2016-11-19 15:10_

So... I want to fix this issue but I'm not sure what the best way is. I understand the _technical_ issues at play, but I'm concerned more about the user experience here. Let me describe what I think is the problem.

Today, if I'm on Linux and I want to download a Linux binary for ripgrep, I click on the "releases" link and there is exactly one choice that makes sense: `ripgrep-0.2.9-x86_64-unknown-linux-musl.tar.gz`.

In the future, if we add different downloads for various flavors of SIMD support, then will be multiple choices and none of them will be obvious. For example, you might have to choose between `ripgrep-0.2.9-x86_64-unknown-linux-musl.tar.gz` and `ripgrep-0.2.9-x86_64-unknown-linux-musl-ssse3.tar.gz`. As a user of command line search tools, I have no idea what SIMD is or what SSSE3 means or whether my system supports it. Therefore, I will choose `ripgrep-0.2.9-x86_64-unknown-linux-musl.tar.gz`. (Which is likely the wrong choice in the vast majority of cases.)

How can I fix this? I could add some notes about this in the release page, but I feel like that will be ineffective.


---

_Label `question` added by @BurntSushi on 2016-11-19 15:10_

---

_Comment by @acmay on 2017-01-10 07:12_

It is not unheard of to have a program do run time detection of the supported instruction set and pick the correct functions as needed. Openssl with AES instruction support is one example. VLC and other video players are another good source of examples. Here they actually fork and test the instruction works since they don't trust the CPU ID bits to be correct. https://mailman.videolan.org/pipermail/vlc-devel/2009-September/066544.html

So the general solution may be to create one binary that detects and loads the correct arch specific module. I have no idea how to do that in rust, but I would like to hope it is possible without too many hoops. And hopefully it is just a small subset of the code that really needs the instructions for the performance boost.

---

_Comment by @BurntSushi on 2017-01-10 12:08_

@acmay Ah, thanks for the update! Yup, Rust has a few cpuid crates that we could use to avoid running SSSE3 code if it's not available. The problem is actually a bit more sinister then that though. In particular, if I compile a binary using the equivalent of `-mssse3`, then that enables SSSE3 based optimizations *everywhere*.

The only way to solve this problem for real is using target specific function attributes. (Which I just recently added to the Rust compiler.) This allows one to use SSSE3 vendor intrinsics on a function-by-function level without actually passing `-mssse3` as a compile-time flag. We can then use, e.g., cpuid to dispatch.

---

_Comment by @dpwiz on 2017-02-02 13:40_

I have an AMD CPU and get `Illegal instruction`s too.

---

_Comment by @BurntSushi on 2017-02-02 13:41_

@wiz Can you show the output of `cat /proc/cpuinfo`? Whether it's AMD or not shouldn't matter.

---

_Comment by @dpwiz on 2017-02-02 13:52_

Sure,
```
processor       : 0
vendor_id       : AuthenticAMD
cpu family      : 16
model           : 10
model name      : AMD Phenom(tm) II X6 1055T Processor
stepping        : 0
microcode       : 0x10000bf
cpu MHz         : 800.000
cache size      : 512 KB
physical id     : 0
siblings        : 6
core id         : 0
cpu cores       : 6
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 6
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm 3dnowext 3dnow constant_tsc rep_good nopl nonstop_tsc extd_apicid aperfmperf pni monitor cx16 popcnt lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt cpb hw_pstate vmmcall npt lbrv svm_lock nrip_save pausefilter
bugs            : tlb_mmatch apic_c1e fxsave_leak sysret_ss_attrs
bogomips        : 5625.70
TLB size        : 1024 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management: ts ttp tm stc 100mhzsteps hwpstate cpb
```

According to wikipedia SSSE3 instructions are available only on APUs and maybe newer chips.

---

_Comment by @BurntSushi on 2017-02-02 13:59_

@wiz OK, so it's the same issue as others.

For others: there's no need to chime in if you got this error and you know your CPU doesn't support SSSE3. It's a known issue. The current work-around requires you to compile from source.

---

_Comment by @dpwiz on 2017-02-02 14:10_

I know it is the same one. It's just dumping good bunch of AMD (those are mostly affected) users seems wrong.

---

_Comment by @BurntSushi on 2017-02-02 14:14_

I agree. That's why this is an open issue.

---

_Comment by @gblues on 2017-02-24 20:23_

I'm not a user of ripgrep, but I came across this while looking for something else entirely and thought of something that might help you out.

Suggestion: package all versions of the binary in a single RPM and use an install-time script to figure out which one to actually use.

ex:
1. install PREFIX/bin/ripgrep-ssse3, PREFIX/bin/ripgrep-ssse2, etc. 
2. Write an install script detects actual CPU capabilities
3. create symlink for PREFIX/bin/ripgrep to the appropriate binary based on detected capabilities

This avoids having to manage complex state in a single binary, potentially having the performance gains from using SIMD lost by the capability checks, at the cost of a larger package.

---

_Comment by @BurntSushi on 2017-02-24 20:32_

@gblues Unfortunately, RPMs only work on certain Linux distros, which makes it a non-starter. "Make packages for every Linux distro" is also a non-starter.

---

_Comment by @pdf on 2017-08-25 06:58_

@BurntSushi perhaps keep the default build as the one that includes the latest SIMD, and denote the alternative builds with something like `nosse3`, with a note about the errors users of such processors may encounter, then at least google can solve their problem.  This way, most users get the additional optimizations, but the option is there for those with unsupported processors, that they don't have to work out a whole rust toolchain to get a build done.

Worst case for users of modern processors who download the wrong version though is that they get a slightly less optimized build, right?  Doesn't seem like a much of a big deal.

---

_Closed by @BurntSushi on 2018-03-13 03:21_

---

_Comment by @jamesmudd on 2018-08-24 13:11_

In the release notes it says
```
BUG #135:
Release portable binaries that conditionally use SSSE3, AVX2, etc., at
runtime.
```
I have downloaded the https://github.com/BurntSushi/ripgrep/releases/download/0.9.0/ripgrep-0.9.0-x86_64-unknown-linux-musl.tar.gz build but when I run it, it doesn't seem to take advantage of the CPU features available e.g.
```
$ rg --version       
ripgrep 0.9.0 (rev 6799dcfc0e)
-SIMD -AVX
```
The output of ``cat /proc/cpuinfo``
```
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 63
model name	: Intel(R) Xeon(R) CPU E5-1630 v3 @ 3.70GHz
stepping	: 2
microcode	: 57
cpu MHz		: 1200.000
cache size	: 10240 KB
physical id	: 0
siblings	: 8
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 15
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good xtopology nonstop_tsc aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm ida arat epb xsaveopt pln pts dtherm invpcid_single pti retpoline tpr_shadow vnmi flexpriority ept vpid fsgsbase bmi1 avx2 smep bmi2 erms invpcid cqm cqm_llc cqm_occup_llc
bogomips	: 7382.80
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
```
So the CPU an ``Intel E5-1630 v3`` seems to support AVX and AVX2 but it looks like ripgrep isn't seeing that? Also in ripgrep 0.9.0 it now seems to detect no support for SIMD which used to work on the same system in 0.8.1
```
$ rg --version 
ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX
```
Maybe I have misunderstood but I expected that 0.9.0 would detect and use SMID and AVX?

---

_Comment by @BurntSushi on 2018-08-24 13:16_

@jamesmudd It should be, yes. The `-SIMD -AVX` refer to compile time options, neither of which are now enabled in the binary releases. Previously, the SSSE3 feature was enabled at compile time, which is why `0.8.1` shows `+SIMD`, and is exactly what caused binaries to be non-portable, and therefore, caused  the bug reported in this issue.

The build instructions in the README indicate that some SIMD features are enabled at compile time (line counting and transcoding, specifically) while others are enabled at runtime (regex matching). The ideal is that they are all enabled at runtime, but we'll have to wait for the maintainers of the respective libraries to catch up and add support for runtime detection.

The misleading output of `--version` is fixed on master, which now shows both compile time and run time feature detection:

```
$ rg --version
ripgrep 0.9.0 (rev 1bb8b7170f)
+SIMD +AVX (compiled)
+SIMD +AVX (runtime)
```

The only way to enable compile time SIMD/AVX is to build ripgrep yourself.

See also: https://github.com/BurntSushi/ripgrep/issues/1013

---

_Comment by @jamesmudd on 2018-08-24 15:30_

Thanks for the explanation sorry I didn't see the other bug. Ripgrep is a great product keep up the good work!

---

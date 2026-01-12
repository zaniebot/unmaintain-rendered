```yaml
number: 283
title: type rg help, results core dump
type: issue
state: closed
author: xahlee
labels: []
assignees: []
created_at: 2016-12-20T04:29:35Z
updated_at: 2016-12-20T13:51:14Z
url: https://github.com/BurntSushi/ripgrep/issues/283
synced_at: 2026-01-12T16:13:21Z
```

# type rg help, results core dump

---

_@xahlee_

type
`rg help`
results
Illegal instruction (core dumped)

this is on Ubuntu 16.04.1 LTS
Linux xah-p6813w 4.4.0-53-generic #74-Ubuntu SMP Fri Dec 2 15:59:10 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
using binary
ripgrep-0.3.2-x86_64-unknown-linux-musl

---

_Comment by @BurntSushi on 2016-12-20 10:35_

Please show the output of `cat /proc/cpuinfo`. My guess is that you have a cpu without ssse3 support. Therefore you cannot use the distributed binaries.

---

_Comment by @xahlee on 2016-12-20 11:01_

here's cpuinfo. You are right, probably no ssse3 support. Thank you.

```
$ cat /proc/cpuinfo
processor	: 0
vendor_id	: AuthenticAMD
cpu family	: 16
model		: 5
model name	: AMD Athlon(tm) II X4 645 Processor
stepping	: 3
microcode	: 0x10000c8
cpu MHz		: 800.000
cache size	: 512 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 5
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm 3dnowext 3dnow constant_tsc rep_good nopl nonstop_tsc extd_apicid pni monitor cx16 popcnt lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt hw_pstate vmmcall npt lbrv svm_lock nrip_save
bugs		: tlb_mmatch fxsave_leak sysret_ss_attrs
bogomips	: 6200.49
TLB size	: 1024 4K pages
clflush size	: 64
cache_alignment	: 64
address sizes	: 48 bits physical, 48 bits virtual
power management: ts ttp tm stc 100mhzsteps hwpstate

processor	: 1
vendor_id	: AuthenticAMD
cpu family	: 16
model		: 5
model name	: AMD Athlon(tm) II X4 645 Processor
stepping	: 3
microcode	: 0x10000c8
cpu MHz		: 800.000
cache size	: 512 KB
physical id	: 0
siblings	: 4
core id		: 1
cpu cores	: 4
apicid		: 1
initial apicid	: 1
fpu		: yes
fpu_exception	: yes
cpuid level	: 5
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm 3dnowext 3dnow constant_tsc rep_good nopl nonstop_tsc extd_apicid pni monitor cx16 popcnt lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt hw_pstate vmmcall npt lbrv svm_lock nrip_save
bugs		: tlb_mmatch fxsave_leak sysret_ss_attrs
bogomips	: 6200.49
TLB size	: 1024 4K pages
clflush size	: 64
cache_alignment	: 64
address sizes	: 48 bits physical, 48 bits virtual
power management: ts ttp tm stc 100mhzsteps hwpstate

processor	: 2
vendor_id	: AuthenticAMD
cpu family	: 16
model		: 5
model name	: AMD Athlon(tm) II X4 645 Processor
stepping	: 3
microcode	: 0x10000c8
cpu MHz		: 1900.000
cache size	: 512 KB
physical id	: 0
siblings	: 4
core id		: 2
cpu cores	: 4
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu_exception	: yes
cpuid level	: 5
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm 3dnowext 3dnow constant_tsc rep_good nopl nonstop_tsc extd_apicid pni monitor cx16 popcnt lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt hw_pstate vmmcall npt lbrv svm_lock nrip_save
bugs		: tlb_mmatch fxsave_leak sysret_ss_attrs
bogomips	: 6200.49
TLB size	: 1024 4K pages
clflush size	: 64
cache_alignment	: 64
address sizes	: 48 bits physical, 48 bits virtual
power management: ts ttp tm stc 100mhzsteps hwpstate

processor	: 3
vendor_id	: AuthenticAMD
cpu family	: 16
model		: 5
model name	: AMD Athlon(tm) II X4 645 Processor
stepping	: 3
microcode	: 0x10000c8
cpu MHz		: 800.000
cache size	: 512 KB
physical id	: 0
siblings	: 4
core id		: 3
cpu cores	: 4
apicid		: 3
initial apicid	: 3
fpu		: yes
fpu_exception	: yes
cpuid level	: 5
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm 3dnowext 3dnow constant_tsc rep_good nopl nonstop_tsc extd_apicid pni monitor cx16 popcnt lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt hw_pstate vmmcall npt lbrv svm_lock nrip_save
bugs		: tlb_mmatch fxsave_leak sysret_ss_attrs
bogomips	: 6200.49
TLB size	: 1024 4K pages
clflush size	: 64
cache_alignment	: 64
address sizes	: 48 bits physical, 48 bits virtual
power management: ts ttp tm stc 100mhzsteps hwpstate
```

---

_Comment by @BurntSushi on 2016-12-20 11:48_

Yup, that's the problem. The binaries I ship are compiled with SSSE3 support, which I realize means they aren't guaranteed to work on every x86_64 CPU. If you compile ripgrep from source with `cargo build --release`, then you should get a working binary.

This whole issue will be fixed once Rust gets better SIMD support with the ability to dynamically pick the right algorithms at runtime, which I hope will happen in 2017.

---

_Closed by @BurntSushi on 2016-12-20 11:48_

---

_Comment by @xahlee on 2016-12-20 13:44_

Superb work on ripgrep. Thank you again.

---

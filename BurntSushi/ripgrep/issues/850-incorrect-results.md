```yaml
number: 850
title: Incorrect results
type: issue
state: closed
author: abhichandra21
labels:
  - question
assignees: []
created_at: 2018-03-09T03:39:15Z
updated_at: 2018-04-23T23:48:37Z
url: https://github.com/BurntSushi/ripgrep/issues/850
synced_at: 2026-01-12T16:13:22Z
```

# Incorrect results

---

_@abhichandra21_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
+SIMD -AVX

#### What operating system are you using ripgrep on?

SLES 11 SP3

#### Describe your question, feature request, or bug.

Tried to search using grep and ripgrep and the ripgrep results are incorrect. 
Incorrect
I was trying to do some performance testing between grep and ripgrep and the results shown by ripgrep are not correct. I'm searching through log files ~20G in size. Changing options by removing "-F" or using a single cpu by adding -j1 didn't make any difference. 

#### If this is a bug, what are the steps to reproduce the behavior?

These log files are proprietary, so I can't provide the actual logs.

#### If this is a bug, what is the actual behavior?

Additional output using the --debug flag is following:
Literal {
    chars: [
        'S',
        'u',
        'c',
        'c',
        'e',
        's',
        's',
        'f',
        'u',
        'l',
        'l',
        'y',
        ' ',
        's',
        'e',
        'n',
        't',
        ' ',
        't',
        'o'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(Successfully sent to)], limit_size: 250, limit_class: 10 }

#### If this is a bug, what is the expected behavior?
Show correct counts for the string I was searching.

Output:
Incorrect

```
[user1@test@linux 20180308]# time /opt/emageon/var/achandra/rg -Fc "Successfully sent to" 201803080* 201803081* 2018030820.log | sort
2018030800.log:301
2018030801.log:55489
2018030802.log:10139
2018030803.log:2881
2018030804.log:16529
2018030805.log:2126
2018030806.log:3772
2018030807.log:10811
2018030808.log:1670
2018030809.log:524
2018030810.log:10
2018030811.log:154
2018030812.log:2965
2018030813.log:2435
2018030814.log:3133
2018030815.log:654
2018030816.log:644
2018030817.log:2206
2018030818.log:312
2018030819.log:2458
2018030820.log:962

real    1m57.627s
user    0m20.769s
sys     0m14.877s
[user1@test@linux 20180308]#

```
Correct
```

[user1@test@linux 20180308]# time grep -Fc "Successfully sent to" 201803080* 201803081* 2018030820.log
2018030800.log:46377
2018030801.log:65685
2018030802.log:40223
2018030803.log:41577
2018030804.log:39665
2018030805.log:21650
2018030806.log:29316
2018030807.log:52661
2018030808.log:108611
2018030809.log:129357
2018030810.log:200449
2018030811.log:194566
2018030812.log:147773
2018030813.log:181545
2018030814.log:176097
2018030815.log:157315
2018030816.log:170310
2018030817.log:22196
2018030818.log:238030
2018030819.log:155504
2018030820.log:112215

real    37m26.343s
user    2m12.048s
sys     1m58.763s
[user1@test@linux 20180308]#
```

---

_Comment by @abhichandra21 on 2018-03-09 03:49_

I also get OOM errors frequentely

```
[user1@test@linux 20180308]# /opt/emageon/var/achandra/rg -cF "Successfully sent to" 2018030723.log  2018030801.log  2018030803.log  2018030805.log  2018030807.log  2018030809.log  2018030811.log  2018030813.log
Out of memory (os error 12)
Out of memory (os error 12)
Out of memory (os error 12)
Out of memory (os error 12)
2018030723.log:5

```

---

_Comment by @BurntSushi on 2018-03-09 12:06_

@abhichandra21 Could you try with `-j1 --no-mmap`?

Otherwise, if you can't provide enough data to reproduce this issue, then I'm afraid there really isn't anything I can do.

---

_Comment by @abhichandra21 on 2018-03-09 15:55_

Same results, but the counts are always random.
I totally understand that without the sample you won't be able to investigate much. I wanted to know if you had seen similar behavior before. Thanks for your time.

---

_Comment by @BurntSushi on 2018-03-09 16:12_

@abhichandra21 No, definitely not. Incorrect results like this on a simple literal query is a *serious* bug. If I could reproduce it, I would try to fix it as quickly as possible. My guess is that there's something else going on that we're not accounting for and that the search itself isn't buggy. Is there anything else peculiar about your environment? If you're willing to work with me on this, then maybe we can try to debug this:

1. Does the same problem occur if you're searching only one of these log files?
2. If so, let's throw out the multiple file case and just focus on that one file. Does the same problem occur if you cut the file in half?
3. If so, does it occur if you cut the file in half again? And so on.
4. Do other queries suffer the same problem? How far can you chop the query down and still get results inconsistent with grep?
5. Have you tried using the `-a/--text` flag? Maybe the difference could be accounted for based on binary file detection.

If you repeat that process and get the file down to a reasonable size, then maybe it's possible to sanitize it and share it. I'm also amenable to other forms of sharing. e.g., You could encrypt the file with my public key and I could promise not to share it, but obviously I understand that's a long shot. :-)

---

_Comment by @abhichandra21 on 2018-03-09 16:32_

I definitely want to help because I really want to use the tool if possible. It helps me tremendously in troubleshooting issues in the production environment where I have to deal with huge log files because of the nature of the application.
Please find my comments below:

Does the same problem occur if you're searching only one of these log files? No, searching in one file has always worked, except today.

```
[user1@test@linux 20180308]# /opt/emageon/var/achandra/rg -ac "Successfully sent to" 2018030810.log
Out of memory (os error 12)
[user1@test@linux 20180308]# file 2018030810.log
2018030810.log: ASCII news text, with very long lines, with CR, LF line terminators
[user1@test@linux 20180308]#
```

If so, let's throw out the multiple file case and just focus on that one file. Does the same problem occur if you cut the file in half? I keep trimming the files down last night and I got the result if I had 2-3 files.
If so, does it occur if you cut the file in half again? And so on.
Do other queries suffer the same problem? How far can you chop the query down and still get results inconsistent with grep? With my initial testing, I was testing with just one word(Router).
Have you tried using the -a/--text flag? Maybe the difference could be accounted for based on binary file detection. I just did and I kept running into OOM.

Here is the free -m output

```
             total       used       free     shared    buffers     cached
Mem:         12040      11774        266          0          0      10081
-/+ buffers/cache:       1692      10348
Swap:         4101         23       4078
```
The logs have very sensitive data that I can not share with anyone who's not authorized. 

---

_Comment by @BurntSushi on 2018-03-09 16:39_

@abhichandra21 Whenever you get an OOM error, can you use `--no-mmap` flag and see if that prevents the OOM?

I'm finding it somewhat hard to interpret your response btw. If it's possible to be clearer with the exact commands and exact output, that would help a lot. Otherwise, I basically have to guess.

---

_Comment by @BurntSushi on 2018-03-09 16:39_

@abhichandra21 Also, what is the output of `lscpu` on the machine you're running this on?

---

_Comment by @abhichandra21 on 2018-03-09 16:54_

I ran strace on the command and realized that I should have used --no-mmap. With --no-mmap it seems to work for one file and gives correct results.
```
[user1@test@linux 20180308]# time /opt/emageon/var/achandra/rg --no-mmap -j1 -Fac "Successfully sent to" 2018030814.log
176097

real    2m45.485s
user    0m20.013s
sys     0m8.801s
```

`lscpu `output:
```
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                12
On-line CPU(s) list:   0-11
Thread(s) per core:    1
Core(s) per socket:    6
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 37
Stepping:              1
CPU MHz:               2299.998
BogoMIPS:              4599.99
Hypervisor vendor:     VMware
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              46080K
NUMA node0 CPU(s):     0-5
NUMA node1 CPU(s):     6-11

```

I'm running on multiple files now by increasing one file at a time and I will let you know when it starts failing. I wanted to share the results I had so far.

---

_Comment by @BurntSushi on 2018-03-09 16:58_

That's interesting. I'm really surprised that memory maps are leading to OOMs, but it's likely because I don't understand something about them. For example, if overcommit isn't enabled, and a file backed memory map requests a size greater than available RAM, then perhaps that is what would cause the out-of-memory error. In particular, I would consider that a bug in ripgrep, since it should handle that error and switch to normal read calls. We already do that for certain types of errors: https://github.com/BurntSushi/ripgrep/blob/9163aaac277f6db9938f464d78d212a0e8997c59/src/worker.rs#L351 I think we need to add `ENOMEM` to that list of errors to check for.

---

_Comment by @abhichandra21 on 2018-03-09 17:21_

Worked fine with 3 files but with no performance gain. It was actually slower than grep. 
```
[user1@test@linux 20180308]# time grep -Fac "Successfully sent to" 2018030814.log 2018030815.log 2018030816.log
2018030814.log:176097
2018030815.log:157315
2018030816.log:170310

real    6m53.296s
user    0m27.186s
sys     0m24.146s
[user1@test@linux 20180308]#
```

```
[user1@test@linux 20180308]# time /opt/emageon/var/achandra/rg --no-mmap -Fac "Successfully sent to" 2018030814.log 2018030815.log 2018030816.log
2018030815.log:157315
2018030816.log:170310
2018030814.log:176097

real    7m40.617s
user    1m10.088s
sys     0m39.346s
[user1@test@linux 20180308]#
```

```
-rw-r--r-- 1 user1 users 14G Mar  8 15:00 2018030814.log
-rw-r--r-- 1 user1 users 14G Mar  8 16:00 2018030815.log
-rw-r--r-- 1 user1 users 13G Mar  8 17:00 2018030816.log
```
I'm now running the command with all files to see if I can reproduce again

---

_Comment by @BurntSushi on 2018-03-09 17:24_

Yeah I mean if these files are bigger than available RAM, then there probably isn't going to be much difference in performance. Or more likely, the difference will be variable since it will just be blocked on disk I/O.

I wonder if there is a bug in the counts. Could you try ripgrep 0.7.x? Or another way would be `rg --no-mmap -Fac "..." a.log b.log c.log | wc -l` and compare that result to the equivalent grep command.

---

_Comment by @abhichandra21 on 2018-03-09 17:31_

Where can I get the rpm from? I thought I downloaded the latest.

---

_Comment by @BurntSushi on 2018-03-09 17:32_

@abhichandra21 ripgrep 0.7.x is an _older_ version of ripgrep. I'm asking to check and see if there was a regression.

I don't know anything about RPMs, sorry. You can download statically compiled releases though: https://github.com/BurntSushi/ripgrep/releases/tag/0.7.1

---

_Comment by @BurntSushi on 2018-03-09 17:33_

Also, could you show the full output of `lscpu`? Or did you do that? On my system, there is a `Flags` section, which is what I wanted to see:

```
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  1
Core(s) per socket:  4
Socket(s):           1
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               60
Model name:          Intel(R) Core(TM) i5-4590 CPU @ 3.30GHz
Stepping:            3
CPU MHz:             3298.371
CPU max MHz:         3700.0000
CPU min MHz:         800.0000
BogoMIPS:            6596.58
Virtualization:      VT-x
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            6144K
NUMA node0 CPU(s):   0-3
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand lahf_lm abm cpuid_fault epb invpcid_single pti tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm xsaveopt dtherm ida arat pln pts
```

---

_Comment by @abhichandra21 on 2018-03-09 17:47_

`lscpu` doesn't show the flags for me but here is what I have from `cpuinfo`
```
fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts mmx fxsr sse sse2 ss ht syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology tsc_reliable nonstop_tsc aperfmperf pni pclmulqdq ssse3 cx16 sse4_1 sse4_2 x2apic popcnt aes hypervisor lahf_lm ida arat epb pln pts dtherm
``` 
I got the binary, I will test it when I can. But if I have to disable `mmap` and has performance implications, then I may have to stick with `grep`. All I am looking for is a faster way to look through files.

---

_Comment by @BurntSushi on 2018-03-09 17:51_

> All I am looking for is a faster way to look through files.

If you're searching files that are bigger than available memory, then your speed will be limited by disk regardless of what tool you use (assuming it isn't doing something very dumb :P).

Note that you don't *have* to disable memory map. It sounds like your system is configured in such a way where it doesn't permit memory mapping a file bigger than memory. There ain't nothin ripgrep can do about that. ;-)

---

_Comment by @BurntSushi on 2018-03-09 18:00_

Your CPU flags look OK.

---

_Label `question` added by @BurntSushi on 2018-03-10 13:04_

---

_Comment by @BurntSushi on 2018-03-10 13:14_

I've fixed the memory map/OOM bug. See #852.

---

_Comment by @BurntSushi on 2018-04-23 23:48_

I can't reproduce the problems in this bug report. I'd love to fix them or at least understand them, but we need more data and a better reproduction.

---

_Closed by @BurntSushi on 2018-04-23 23:48_

---

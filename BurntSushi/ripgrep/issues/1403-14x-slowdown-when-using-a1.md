```yaml
number: 1403
title: 14x slowdown when using -A1 
type: issue
state: closed
author: aslushnikov
labels:
  - duplicate
assignees: []
created_at: 2019-10-14T06:58:44Z
updated_at: 2019-10-14T22:44:39Z
url: https://github.com/BurntSushi/ripgrep/issues/1403
synced_at: 2026-01-12T16:13:23Z
```

# 14x slowdown when using -A1 

---

_@aslushnikov_

#### What version of ripgrep are you using?

```
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

I used github release sources.

#### What operating system are you using ripgrep on?

OSX/Linux

#### If this is a bug, what are the steps to reproduce the behavior?

I suspect my exact setup does not matter that much, but still - here it is:

1. Shallow-clone webkit repository: `git clone --depth=1 --single-branch https://github.com/webkit/webkit`.
2. `cd` to the root of webkit repository
3. run `time rg InspectorDOMAgent`
4. run `time rg -A1 InspectorDOMAgent` and compare time with step (3)

For me, `ripgrep` is 14 times slower when using the -A1 option - on both mac and linux:

```
aslushnikov:~/prog/webkit(master)$ time rg InspectorDOMAgent > /dev/null

real    0m1.424s
user    0m5.622s
sys     0m6.904s
aslushnikov:~/prog/webkit(master)$ time rg -A1 InspectorDOMAgent > /dev/null

real    0m18.790s
user    2m49.995s
sys     0m21.041s
```

I find this weird - is this expected behavior?

P.S. Thank you for the `ripgrep`! ❤️

---

_Comment by @BurntSushi on 2019-10-14 17:49_

I can't reproduce on master:

```
$ time rg InspectorDOMAgent > /dev/null

real    0.510
user    2.585
sys     3.091
maxmem  109 MB
faults  0

$ time rg InspectorDOMAgent -A1 > /dev/null

real    0.521
user    2.571
sys     3.287
maxmem  96 MB
faults  0
```

The repository is 4.7GB, and ripgrep reports (via `-q --stats`) that it's searching through 1.8GB of that. That doesn't seem like that much to me, but if your machine has a small amount of RAM, then it's possible that you're just benchmarking disk access.

Otherwise, I can't think of any plausible reason why adding `-A1` would slow ripgrep down by the amount you're observing. It's almost certain that there is some other cause at work here.

Can you give me more information about your environment? Are you actually able to reliably reproduce this issue on _both_ macOS and Linux? Also, why are you using `ripgrep 11.0.1` instead of `ripgrep 11.0.2`?

Ah okay, it looks like this bug has been fixed on master. If I compile `ripgrep 11.0.1`, then I can reproduce your bug:

```
$ time rg InspectorDOMAgent > /dev/null

real    3.720
user    38.309
sys     5.084
maxmem  109 MB
faults  0

$ time rg InspectorDOMAgent -A1 > /dev/null

real    36.178
user    6:53.61
sys     13.603
maxmem  117 MB
faults  0
```

This was almost certainly fixed in: https://github.com/BurntSushi/ripgrep/commit/813c676ecac053559a3b1b72034e0c34c8ed7e06

---

_Closed by @BurntSushi on 2019-10-14 17:49_

---

_Label `duplicate` added by @BurntSushi on 2019-10-14 17:49_

---

_Comment by @aslushnikov on 2019-10-14 21:50_

Thank you for the investigation. If I understand correctly, the fix didn't make it to the 11.0.2. Is a new release scheduled?

---

_Comment by @aslushnikov on 2019-10-14 22:08_

Apologies if there's some release schedule that I don't know about - I'd appreciate a pointer.

---

_Comment by @BurntSushi on 2019-10-14 22:44_

The fix is not in any release, no. If you need it now, then build ripgrep from source.

> Is a new release scheduled?

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

---

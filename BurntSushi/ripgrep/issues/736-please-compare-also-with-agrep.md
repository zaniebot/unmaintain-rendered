```yaml
number: 736
title: "Please compare also with \"agrep\""
type: issue
state: closed
author: Wikinaut
labels:
  - question
assignees: []
created_at: 2018-01-08T09:00:36Z
updated_at: 2018-01-09T00:58:51Z
url: https://github.com/BurntSushi/ripgrep/issues/736
synced_at: 2026-01-12T16:13:22Z
```

# Please compare also with "agrep"

---

_@Wikinaut_

See https://github.com/Wikinaut/agrep

Agrep is an older program but using very fast algorithms (and offers "approximate" matching in addition to standard regexp).
  

---

_Renamed from "pls. compare also with "apgrep"" to "pls. compare also with "agrep"" by @Wikinaut on 2018-01-08 09:00_

---

_Renamed from "pls. compare also with "agrep"" to "Please compare also with "agrep"" by @Wikinaut on 2018-01-08 09:01_

---

_Label `icebox` added by @BurntSushi on 2018-01-08 12:30_

---

_Label `question` added by @BurntSushi on 2018-01-08 12:30_

---

_Comment by @BurntSushi on 2018-01-08 12:46_

It is interestingly faster than GNU grep in some cases, but doesn't quite beat ripgrep:

```
$ time rg -i 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en | wc -l
642

real    0m0.381s
user    0m0.320s
sys     0m0.063s
$ time agrep -i 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en | wc -l
643

real    0m0.494s
user    0m0.349s
sys     0m0.147s
$ time egrep -i 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en | wc -l
642

real    0m2.087s
user    0m1.999s
sys     0m0.090s
```

But agrep will be hard to benchmark. For one, it doesn't appear to support a case sensitive feature. e.g., `agrep foo` and `agrep -i foo` appear to do the same thing, and I don't see anything in the man page to suggest it has a flag to disable this. It does have literal search (`-k`), but that doesn't seem to disable the case insensitive search.

Secondly, it doesn't seem to be Unicode-aware, which will negates some of the more interesting/extreme benchmarks. For example, `rg -i 'Шерлок Холмс' OpenSubtitles2016.raw.ru` reports 604 matches (as does the equivalent `egrep` command), but `agrep` reports 584 matches, which is what case sensitive ripgrep and egrep searches report. In other words, there is no support for Unicode case folding.

Thirdly, agrep seems to just give up entirely on reasonable patterns that it thinks are too long. For example:

```
$ agrep 'Sherlock Holmes|John Watson|Irene Adler|Inspector Lestrade|Professor Moriarty' OpenSubtitles2016.raw.sample.en
agrep: regular expression too long
0
```

There doesn't seem to be any way to easily increase the limit imposed here. This, once again, negates several benchmarks. I tried to widdle the regex down, but even `Sherlock Holmes|John Watson|Irene Adler` is too long. `Sherlock Holmes|John Watson` however is apparently OK. This is kind of a ridiculous restriction IMO, and I can't imagine anyone using this tool for general purpose use, and instead would seemingly only use it for its fuzzy searching capabilities, when needed. Note that even with the shorter pattern, `Sherlock Holmes|John Watson`, ripgrep is an order of magnitude faster than agrep because of its SIMD algorithms.

I think there maybe exists an interesting benchmark that involves agrep---since it does seem to beat GNU grep in some restricted cases---but the benchmark I setup for ripgrep probably isn't it. It would be disqualified from many of them.

---

_Closed by @BurntSushi on 2018-01-08 12:46_

---

_Label `icebox` removed by @BurntSushi on 2018-01-09 00:58_

---

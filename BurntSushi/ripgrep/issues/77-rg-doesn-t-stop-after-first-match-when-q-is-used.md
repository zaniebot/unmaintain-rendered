```yaml
number: 77
title: "rg doesn't stop after first match when -q is used"
type: issue
state: closed
author: 0xmohit
labels:
  - bug
assignees: []
created_at: 2016-09-25T08:47:27Z
updated_at: 2016-09-27T11:55:06Z
url: https://github.com/BurntSushi/ripgrep/issues/77
synced_at: 2026-01-12T18:23:11Z
```

# rg doesn't stop after first match when -q is used

---

_@0xmohit_

Example:

```
$ seq 100000 > t
$ time grep -q 1 t

real    0m0.003s
user    0m0.000s
sys     0m0.000s
$ time rg -q 1 t

real    0m0.139s
user    0m0.124s
sys     0m0.012s
$ time grep -q a t

real    0m0.003s
user    0m0.000s
sys     0m0.004s
$ rg -q a t

real    0m0.017s
user    0m0.008s
sys     0m0.008s
```

The same was observed while searching in real world code:

```
~/go/src/cmd/compile/internal/gc$ time grep OLITERAL sinit.go 
        case OLITERAL:
                        if e.Expr.Op == OLITERAL {
        case OLITERAL:
                if l.Class == PEXTERN && r.Left.Op == OLITERAL {
                        if e.Expr.Op == OLITERAL {
        return n.Op == OLITERAL && n.Val().Ctype() != CTNIL
                        if n.Op == OARRAYLIT && index.Op != OLITERAL {
        case OLITERAL:
        case OLITERAL:
        case OLITERAL:

real    0m0.004s
user    0m0.004s
sys     0m0.000s
~/go/src/cmd/compile/internal/gc$ time rg OLITERAL sinit.go 
302:    case OLITERAL:
345:                    if e.Expr.Op == OLITERAL {
381:    case OLITERAL:
416:            if l.Class == PEXTERN && r.Left.Op == OLITERAL {
450:                    if e.Expr.Op == OLITERAL {
583:    return n.Op == OLITERAL && n.Val().Ctype() != CTNIL
645:                    if n.Op == OARRAYLIT && index.Op != OLITERAL {
654:    case OLITERAL:
1278:   case OLITERAL:
1391:   case OLITERAL:

real    0m0.028s
user    0m0.028s
sys     0m0.000s
```


---

_Comment by @BurntSushi on 2016-09-25 13:05_

Your first case is pretty pathological, but interesting. The problem is that you're searching for a single byte in a file where that byte is extremely common. It's worth optimizing, but pretty low proirity.

Your second case looks like it's on a tiny sample. There's another issue for looking at making startup time faster, since that's almost assuredly what you're observing.

Certainly, I don't see any relationship in terms of performance between your examples.


---

_Comment by @0xmohit on 2016-09-25 13:28_

> Certainly, I don't see any relationship in terms of performance between your examples.

The intent was to illustrate the performance issue when searching in a single file; those were intended to be different.

Moreover, the first one specified `-q`; the only byte being searched for was the first one in the input.


---

_Comment by @BurntSushi on 2016-09-25 13:44_

@0xmohit Oh! I missed the `-q` flag and jumped straight to unrelated things. :-) Thanks for pointing that out. I don't think `rg` is quitting after it finds the first match.


---

_Comment by @BurntSushi on 2016-09-25 14:07_

But yeah, in the second case, the issue is definitely startup time. I was able to reproduce it.I'm going to lump that part of your issue in with #33, and change the first part to "fix the `-q` flag."


---

_Renamed from "rg is slow when operating on a single file" to "rg doesn't stop after first match when -q is used" by @BurntSushi on 2016-09-25 14:08_

---

_Comment by @BurntSushi on 2016-09-25 14:11_

For reference, if we remove the bug from the benchmark, `rg` does beat `grep`:

```
$ seq 100000000 > t
$ ls -lh
total 848M
-rw-r--r-- 1 andrew users 848M Sep 25 10:09 t
$ time rg 1 t | wc -l
56953280

real    0m3.381s
user    0m3.790s
sys     0m0.370s
$ time grep 1 t | wc -l
56953280

real    0m4.011s
user    0m4.293s
sys     0m0.707s
```

This suggests `rg` isn't actually suffering pathological behavior, and this is just a plain ol' bug.


---

_Label `bug` added by @BurntSushi on 2016-09-25 14:11_

---

_Comment by @BurntSushi on 2016-09-25 14:14_

Clarification, `grep` in ASCII mode is roughly the same as `rg` in Unicode mode:

```
$ time LC_ALL=C grep 1 t | wc -l
56953280

real    0m3.514s
user    0m3.547s
sys     0m0.633s
$ time LC_ALL=en_US.UTF-8 grep 1 t | wc -l
56953280

real    0m3.942s
user    0m3.997s
sys     0m0.627s
```

Benchmarks are hard. :-)


---

_Comment by @0xmohit on 2016-09-25 14:25_

> Benchmarks are hard. :-)

Oh, yes!  Quoting [Benchmark_(computing)#Challenges](https://en.wikipedia.org/wiki/Benchmark_%28computing%29#Challenges):

> Benchmarking is not easy and often involves several iterative rounds in order to arrive at predictable, useful conclusions. Interpretation of benchmarking data is also extraordinarily difficult.


---

_Closed by @BurntSushi on 2016-09-25 19:01_

---

_Comment by @0xmohit on 2016-09-27 11:50_

It seems that this would only stop searching a file after a matched; when searching in a directory, it'll probably still scan all files.

---

Edit: To elaborate:

```
rg -q . dir_with_lots_of_files
```

should probably exit almost immediately.


---

_Comment by @BurntSushi on 2016-09-27 11:55_

@0xmohit Oh... Nice! That should be an easy one to fix. I created #116. Thanks!


---

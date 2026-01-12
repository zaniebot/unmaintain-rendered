```yaml
number: 1760
title: ripgrep is slower than grep when searching for whitespace between words guarded by unicode word boundaries
type: issue
state: closed
author: awalgarg
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-12-10T21:44:21Z
updated_at: 2023-10-10T00:29:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1760
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep is slower than grep when searching for whitespace between words guarded by unicode word boundaries

---

_@awalgarg_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
# pacman -S ripgrep
```

#### What operating system are you using ripgrep on?

```
$ uname -mr
5.9.9-arch1-1 x86_64
```

#### Describe your bug.

When searching a file for a pattern of the following form, ripgrep is significantly slower compared to grep, unless `--no-unicode` is used.

```
\bfoo\b \bbar\b
```

Removing any of the `\b`s above or removing the space in the middle removes this problem. The issue can also be reproduced after replacing the space with other whitespace patterns such as `\t` and `\r`.

#### What are the steps to reproduce the behavior?

```
$ git clone git@github.com:git/git.git
[...]
$ cd git
$ git ls-files | xargs cat > foo
[...]
$ ls -alh foo
-rw-r--r-- 1 user user 36M Dec 11 03:06 foo
$ file foo
foo: UTF-8 Unicode text
$ time grep -ic '\bfoo\b \bbar\b' foo
114

________________________________________________________
Executed in   79.27 millis    fish           external
   usr time   66.16 millis  1167.00 micros   65.00 millis
   sys time   13.09 millis  106.00 micros   12.98 millis

$ time rg -ic '\bfoo\b \bbar\b' foo
114

________________________________________________________
Executed in    1.60 secs   fish           external
   usr time  1589.21 millis  1117.00 micros  1588.09 millis
   sys time    6.66 millis    0.00 micros    6.66 millis

```

With `--no-unicode`, ripgrep is as fast as grep or faster.

---

_Label `bug` added by @BurntSushi on 2020-12-11 13:03_

---

_Comment by @BurntSushi on 2020-12-11 13:12_

Thanks for the detailed and easy to reproduce report! The cause of this is two-fold (repeating what I said in our email exchange):

1. ripgrep enables Unicode by default and a Unicode-aware `\b` (word boundary) has a slower implementation path.
2. ripgrep's `--debug` output shows that _no_ literals are extracted from this regex, even though this looks like a pretty simple case with obvious inner literals. It looks like this occurs because the [inner literal detector gives up completely when it sees a word boundary](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/regex/src/literal.rs#L261).

(1) is hard to fix, although, if you're fine with an ASCII word boundary, then using `--no-unicode` (or `(?-u)\bfoo\b \bbar\b` as the regex) will work around the performance problem since ASCII word boundaries are implemented more efficiently.

(2) should be simpler to fix, although the inner literal detector is pretty hairy at the moment. Perhaps supporting simple cases like this would be easy.

In general though, sadly, regexes with Unicode-aware `\b` will be slower to execute. The literal optimization is just something helps in a lot of cases.

---

_Comment by @BurntSushi on 2023-10-09 23:57_

This will be fixed in the ripgrep 14 release:

```
$ hyperfine "rg-13.0.0 -ic '\bfoo\b \bbar\b' git-3a06386e.txt" "rg -ic '\bfoo\b \bbar\b' git-3a06386e.txt"
Benchmark 1: rg-13.0.0 -ic '\bfoo\b \bbar\b' git-3a06386e.txt
  Time (mean ± σ):      1.034 s ±  0.011 s    [User: 1.030 s, System: 0.004 s]
  Range (min … max):    1.021 s …  1.053 s    10 runs

Benchmark 2: rg -ic '\bfoo\b \bbar\b' git-3a06386e.txt
  Time (mean ± σ):       6.3 ms ±   0.3 ms    [User: 4.6 ms, System: 1.6 ms]
  Range (min … max):     5.6 ms …   7.3 ms    343 runs

Summary
  'rg -ic '\bfoo\b \bbar\b' git-3a06386e.txt' ran
  164.95 ± 7.70 times faster than 'rg-13.0.0 -ic '\bfoo\b \bbar\b' git-3a06386e.txt'
```

This was not fixed by making \b itself faster, but rather, by improving
inner literal extraction. In particular, if the regex doesn't have any
literals extracted, then search time can still be quite slow:

```
$ time rg-13.0.0 -ic '\b[a-z]{3}\b\s\b[a-z]{3}\b' git-3a06386e.txt
57538

real    0.427
user    0.423
sys     0.003
maxmem  46 MB
faults  0
$ time rg -ic '\b[a-z]{3}\b\s\b[a-z]{3}\b' git-3a06386e.txt
57538

real    0.337
user    0.333
sys     0.003
maxmem  46 MB
faults  0
```

But then again, so is grep, because grep doesn't benefit from any
literal optimizations either:

```
$ time grep -E -ic '\b[a-z]{3}\b\s\b[a-z]{3}\b' git-3a06386e.txt
62396

real    1.316
user    1.292
sys     0.007
maxmem  13 MB
faults  7
```

(The count mismatch is quite interesting!)

And if you disable Unicode, then everyone gets faster:

```
$ time LC_ALL=C grep -E -ic '\b[a-z]{3}\b\s\b[a-z]{3}\b' git-3a06386e.txt
58798

real    0.093
user    0.090
sys     0.003
maxmem  13 MB
faults  0

$ time rg-13.0.0 --no-unicode -ic '\b[a-z]{3}\b\s\b[a-z]{3}\b' git-3a06386e.txt
58798

real    0.054
user    0.054
sys     0.000
maxmem  46 MB
faults  0

$ time rg --no-unicode -ic '\b[a-z]{3}\b\s\b[a-z]{3}\b' git-3a06386e.txt
58798

real    0.055
user    0.055
sys     0.000
maxmem  46 MB
faults  0
```

---

_Label `rollup` added by @BurntSushi on 2023-10-10 00:07_

---

_Closed by @BurntSushi on 2023-10-10 00:29_

---

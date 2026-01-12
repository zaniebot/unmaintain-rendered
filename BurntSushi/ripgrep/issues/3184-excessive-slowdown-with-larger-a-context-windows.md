```yaml
number: 3184
title: "Excessive slowdown with larger `-A` context windows?"
type: issue
state: closed
author: d-e-s-o
labels:
  - bug
assignees: []
created_at: 2025-10-13T20:37:49Z
updated_at: 2025-10-14T19:13:20Z
url: https://github.com/BurntSushi/ripgrep/issues/3184
synced_at: 2026-01-12T16:13:25Z
```

# Excessive slowdown with larger `-A` context windows?

---

_@d-e-s-o_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

rg --version
ripgrep 14.1.1

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.

### How did you install ripgrep?

`emerge`

### What operating system are you using ripgrep on?

Gentoo

### Describe your bug.

On large files, `rg` seems to slow down significantly with larger `-A` values.

```
$ ls -lh large_file
> -rwxr-xr-x 1 XXX XXX 9.8G Oct 10 11:35 large_file
```

```sh
$ time cat large_file | rg XXXXX -A99
binary file matches (found "\0" byte around offset 7)

________________________________________________________
Executed in  334.68 millis    fish           external
   usr time  260.39 millis    2.94 millis  257.45 millis
   sys time   75.03 millis    0.99 millis   74.04 millis

$ time cat large_file | rg XXXXX -A999
binary file matches (found "\0" byte around offset 7)

________________________________________________________
Executed in  349.03 millis    fish           external
   usr time  269.92 millis    1.57 millis  268.36 millis
   sys time   91.83 millis    1.02 millis   90.81 millis

$ time cat large_file | rg XXXXX -A9999
binary file matches (found "\0" byte around offset 7)

________________________________________________________
Executed in  667.18 millis    fish           external
   usr time  565.68 millis    1.62 millis  564.06 millis
   sys time  103.22 millis    1.02 millis  102.20 millis

$ time cat large_file | rg XXXXX -A99999
binary file matches (found "\0" byte around offset 7)

________________________________________________________
Executed in    3.39 secs    fish           external
   usr time    3.27 secs    0.73 millis    3.27 secs
   sys time    0.12 secs    2.01 millis    0.12 secs

$ time cat large_file | rg XXXXX -A999999
binary file matches (found "\0" byte around offset 7)

________________________________________________________
Executed in   28.48 secs    fish           external
   usr time   28.29 secs    0.82 millis   28.28 secs
   sys time    0.14 secs    2.01 millis    0.13 secs

$ time cat large_file | rg XXXXX -A9999999
binary file matches (found "\0" byte around offset 7)

________________________________________________________
Executed in  235.28 secs    fish           external
   usr time  234.74 secs    2.70 millis  234.73 secs
   sys time    0.18 secs    1.02 millis    0.18 secs
```
The larger the context window, the longer it takes, it seems.

Compare that to `grep`:
```sh
$ time cat large_file | grep XXXXX -A99
grep: (standard input): binary file matches

________________________________________________________
Executed in    5.54 secs    fish           external
   usr time    0.23 secs    1.46 millis    0.23 secs
   sys time    5.29 secs    2.04 millis    5.28 secs

$ time cat large_file | grep XXXXX -A999
grep: (standard input): binary file matches

________________________________________________________
Executed in    5.56 secs    fish           external
   usr time    0.22 secs    1.00 millis    0.22 secs
   sys time    5.34 secs    2.97 millis    5.34 secs

$ time cat large_file | grep XXXXX -A9999
grep: (standard input): binary file matches

________________________________________________________
Executed in    5.50 secs    fish           external
   usr time    0.24 secs    1.12 millis    0.23 secs
   sys time    5.27 secs    3.08 millis    5.27 secs

$ time cat large_file | grep XXXXX -A99999
grep: (standard input): binary file matches

________________________________________________________
Executed in    5.53 secs    fish           external
   usr time    0.22 secs    0.27 millis    0.22 secs
   sys time    5.30 secs    3.03 millis    5.30 secs

$ time cat large_file | grep XXXXX -A999999
grep: (standard input): binary file matches

________________________________________________________
Executed in    5.36 secs    fish           external
   usr time    0.20 secs    1.32 millis    0.20 secs
   sys time    5.18 secs    2.03 millis    5.18 secs

$ time cat large_file | grep XXXXX -A9999999
grep: (standard input): binary file matches

________________________________________________________
Executed in    5.48 secs    fish           external
   usr time    0.23 secs    0.00 millis    0.23 secs
   sys time    5.25 secs    2.94 millis    5.25 secs
```

This is very unfortunate behavior, as it means that `grep` can, in fact, not rest in peace.

### What are the steps to reproduce the behavior?

See above.

### What is the actual behavior?

It's slow.

### What is the expected behavior?

It should be faster.

---

_Comment by @BurntSushi on 2025-10-13 21:09_

I can't seem to reproduce this. Or at least, not in a way that results in a meaningful difference with GNU grep. Can you please provide an MRE?

Here's what I tried. First, the setup:

```
$ curl -LO 'https://burntsushi.net/stuff/opensubtitles/2018/en/sixteenth.txt.gz'
$ gzip -d sixteenth.txt.gz
$ ls -lh
total 773M
-rw-rw-r-- 1 andrew users 773M Oct 13 16:57 sixteenth.txt
$ (echo ZQZQZQZQZQ && for ((i=0;i<10;i++)); do cat sixteenth.txt; done) > bigger.txt
$ time rg -c ZQZQZQZQZQ bigger.txt
1

real    0.645
user    0.414
sys     0.229
maxmem  7734 MB
faults  0
```

Now `grep` timings:

```
$ (time grep ZQZQZQZQZQ bigger.txt -A999999) | wc -l

real    0.711
user    0.202
sys     0.506
maxmem  29 MB
faults  0
1000000
$ (time grep ZQZQZQZQZQ bigger.txt -A9999999) | wc -l

real    1.055
user    0.514
sys     0.538
maxmem  29 MB
faults  0
10000000
$ (time grep ZQZQZQZQZQ bigger.txt -A99999999) | wc -l

real    4.543
user    3.691
sys     0.845
maxmem  29 MB
faults  0
100000000
$ (time grep ZQZQZQZQZQ bigger.txt -A999999999) | wc -l

real    11.321
user    9.942
sys     1.364
maxmem  29 MB
faults  0
275906531
```

And now ripgrep timings with the same `-A` arguments:

```
$ (time rg ZQZQZQZQZQ bigger.txt -A999999) | wc -l

real    0.645
user    0.428
sys     0.214
maxmem  7734 MB
faults  0
1000000
$ (time rg ZQZQZQZQZQ bigger.txt -A9999999) | wc -l

real    0.975
user    0.742
sys     0.230
maxmem  7734 MB
faults  0
10000000
$ (time rg ZQZQZQZQZQ bigger.txt -A99999999) | wc -l

real    3.943
user    3.513
sys     0.423
maxmem  7734 MB
faults  0
100000000
$ (time rg ZQZQZQZQZQ bigger.txt -A999999999) | wc -l

real    9.781
user    8.928
sys     0.839
maxmem  7735 MB
faults  0
275906531
```

The memory usage reported is high, but that's because of memory mapping. You can disable that and get similar memory usage as GNU grep:

```
$ (time rg ZQZQZQZQZQ bigger.txt --no-mmap -A999999999) | wc -l

real    9.318
user    8.201
sys     1.103
maxmem  29 MB
faults  0
275906531
```

Version info:

```
$ grep -V
grep (GNU grep) 3.12-modified
Copyright (C) 2025 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Mike Haertel and others; see
<https://git.savannah.gnu.org/cgit/grep.git/tree/AUTHORS>.

grep -P uses PCRE2 10.46 2025-08-27

$ rg --version
ripgrep 14.1.1 (rev de2567a4c7)

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
```

I'm running the latest ripgrep built from `master`, but I tried the above commands with the ripgrep 14.1.1 release and there was no meaningful difference.

This is likely the code handling the `-A` flag:

https://github.com/BurntSushi/ripgrep/blob/de2567a4c76fa671005538d6cd841abc44932b9a/crates/searcher/src/searcher/core.rs#L272-L305

I can't spot any obvious sub-optimal behavior here. It's a pretty standard loop-over-lines-and-print.

> This is very unfortunate behavior, as it means that grep can, in fact, not rest in peace.

[That's not what "rip" in "ripgrep" means.](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#intentcountsforsomething)

There are lots of other reasons why grep can't "rest in peace" and those reasons will never go away.

---

_Comment by @BurntSushi on 2025-10-13 21:13_

Oh wait, if I `cat` the file into ripgrep...

```
$ cat bigger.txt | (time rg ZQZQZQZQZQ -A9) | wc -l

real    1.897
user    0.300
sys     0.943
maxmem  29 MB
faults  0
10

$ cat bigger.txt | (time rg ZQZQZQZQZQ -A99) | wc -l

real    1.855
user    0.421
sys     0.840
maxmem  29 MB
faults  0
100

$ cat bigger.txt | (time rg ZQZQZQZQZQ -A999) | wc -l

real    2.009
user    1.265
sys     0.667
maxmem  29 MB
faults  0
1000

$ cat bigger.txt | (time rg ZQZQZQZQZQ -A9999) | wc -l

real    10.459
user    9.894
sys     0.549
maxmem  29 MB
faults  0
10000
```

And indeed, GNU grep does not exhibit a similar slowdown.

WTF.

---

_Label `bug` added by @BurntSushi on 2025-10-13 22:03_

---

_Comment by @BurntSushi on 2025-10-14 18:04_

This was a very subtle problem related to the apparent difference in how much the caller's buffer is filled on `read` syscalls. It's seemingly different when reading from `stdin` versus an explicit file. (Which kind of boggles my mind.) That isn't a problem in and of itself, but it caused a pathological problem in ripgrep where it ended up not amortizing `read` calls as well at it should. See https://github.com/BurntSushi/ripgrep/pull/3185/commits/8bf6f0a2a8fd4d0786561b2901f2b1443ff2d8d4 for more details.

Also, I discovered that GNU grep has a similar problem with `-B/--before-context`:

```
$ cat bigger.txt | (time grep ZQZQZQZQZQ -B9) | wc -l

real    1.568
user    0.170
sys     0.885
maxmem  30 MB
faults  0
1

$ cat bigger.txt | (time grep ZQZQZQZQZQ -B99) | wc -l

real    1.734
user    0.338
sys     0.879
maxmem  30 MB
faults  0
1

$ cat bigger.txt | (time grep ZQZQZQZQZQ -B999) | wc -l

real    2.349
user    1.723
sys     0.620
maxmem  30 MB
faults  0
1

$ cat bigger.txt | (time grep ZQZQZQZQZQ -B9999) | wc -l

real    16.459
user    15.848
sys     0.586
maxmem  30 MB
faults  0
1

$ time grep ZQZQZQZQZQ -B99999 bigger.txt | wc -l

real    1:45.06
user    1:44.12
sys     0.772
maxmem  30 MB
faults  0
1
```

The above pattern occurs regardless of whether you put `bigger.txt` on stdin or whether you search it directly.

ripgrep actually does better in this case (after fixing this bug):

```
$ cat bigger.txt | (time rg ZQZQZQZQZQ -B9) | wc -l

real    1.965
user    0.326
sys     0.814
maxmem  29 MB
faults  0
1

$ cat bigger.txt | (time rg ZQZQZQZQZQ -B99) | wc -l

real    1.941
user    0.423
sys     0.813
maxmem  29 MB
faults  0
1

$ cat bigger.txt | (time rg ZQZQZQZQZQ -B999) | wc -l

real    2.372
user    0.759
sys     0.703
maxmem  30 MB
faults  0
1

$ cat bigger.txt | (time rg ZQZQZQZQZQ -B9999) | wc -l

real    2.638
user    0.895
sys     0.665
maxmem  29 MB
faults  0
1

$ cat bigger.txt | (time rg ZQZQZQZQZQ -B99999) | wc -l

real    5.172
user    3.282
sys     0.748
maxmem  29 MB
faults  0
1
```

With #3185, ripgrep's `-A/--after-context` flag is now quite a bit faster than its `-B/--before-context` flag (GNU grep has this same difference):

```
$ cat bigger.txt | (time rg ZQZQZQZQZQ -A9999) | wc -l

real    1.828
user    0.319
sys     0.744
maxmem  30 MB
faults  0
10000

$ cat bigger.txt | (time rg ZQZQZQZQZQ -A99999) | wc -l

real    1.919
user    0.309
sys     0.780
maxmem  30 MB
faults  0
100000

$ cat bigger.txt | (time rg ZQZQZQZQZQ -A999999) | wc -l

real    1.879
user    0.331
sys     0.759
maxmem  30 MB
faults  0
1000000
```

This is because with `-B`, you need to keep around N lines in your buffer as you read more data. It takes time to "look back" in the buffer to discover those lines. Presumably GNU grep suffers from the same problem given its search times above, but I'm actually not sure why it's so much slower than ripgrep.

---

_Closed by @BurntSushi on 2025-10-14 18:27_

---

_Comment by @d-e-s-o on 2025-10-14 19:13_

Very cool. Thanks for the quick fix!

---

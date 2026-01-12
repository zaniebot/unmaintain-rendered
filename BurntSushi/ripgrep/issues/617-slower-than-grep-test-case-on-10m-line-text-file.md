```yaml
number: 617
title: "Slower than grep: test case on 10M-line text file"
type: issue
state: closed
author: goblin
labels: []
assignees: []
created_at: 2017-09-30T17:54:23Z
updated_at: 2017-12-30T21:00:40Z
url: https://github.com/BurntSushi/ripgrep/issues/617
synced_at: 2026-01-12T16:13:22Z
```

# Slower than grep: test case on 10M-line text file

---

_@goblin_

On your blog you're asking to file an issue with something you can reproduce, so here we go.

Ripgrep 0.60 -AVX -SIMD is slower than GNU grep 2.27 when searching for the last line in a tmpfs-mounted 620MiB file containing 64-character lines, as shown in this log:

```
user@host:~$ sudo mount -t tmpfs none k
user@host:~$ cd k
user@host:~/k$ dd if=/dev/urandom bs=32 count=10000000 | xxd -ps -c32 > file
10000000+0 records in
10000000+0 records out
320000000 bytes (320 MB, 305 MiB) copied, 6.44868 s, 49.6 MB/s
user@host:~/k$ grep --version
grep (GNU grep) 2.27
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Mike Haertel and others, see <http://git.sv.gnu.org/cgit/grep.git/tree/AUTHORS>.
user@host:~/k$ ../ripgrep/target/release/rg --version
ripgrep 0.6.0
-AVX -SIMD
user@host:~/k$ tail -n1 file 
01515049ee5ab42240db3cb6b288f29a68ce9f027219f6e65a63f284ba4d9a76
user@host:~/k$ time grep 01515049ee5ab42240db3cb6b288f29a68ce9f027219f6e65a63f284ba4d9a76 file
01515049ee5ab42240db3cb6b288f29a68ce9f027219f6e65a63f284ba4d9a76

real    0m0.440s
user    0m0.308s
sys     0m0.128s
user@host:~/k$ time ../ripgrep/target/release/rg -N 01515049ee5ab42240db3cb6b288f29a68ce9f027219f6e65a63f284ba4d9a76 file
01515049ee5ab42240db3cb6b288f29a68ce9f027219f6e65a63f284ba4d9a76

real    0m0.728s
user    0m0.708s
sys     0m0.016s
user@host:~/k$ 

```

Subsequent executions show similar timings.

---

_Comment by @hlieberman on 2017-10-15 20:23_

Verified with rg 0.6.0 (-AVX -SIMD) against GNU grep 3.1. 

---

_Comment by @BurntSushi on 2017-10-15 20:46_

The reason is likely because the corpus is using random data. More analysis would be a good idea though of course.

---

_Comment by @BurntSushi on 2017-10-15 21:29_

On second thought, this actually appears to be related to the length of the query string, which probably in turn means that ripgrep is slower because it's not actually using Boyer Moore. On short strings, the benefit of Boyer Moore isn't that great (and is indeed one reason why ripgrep is faster in a large number of common cases), but on longer strings, Boyer Moore means we can skip more of the input, which adds up over time.

Here's an example that adds some indirect evidence of this:

```
$ dd if=/dev/urandom bs=32 count=100000000 | xxd -ps -c32 > file
$ time grep 98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247 file
98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247

real    0m0.306s
user    0m0.180s
sys     0m0.127s
$ time rg -N 98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247 file
98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247

real    0m0.481s
user    0m0.441s
sys     0m0.040s
```

This reproduces the OP's result: ripgrep is slower. Let's start shrinking the length of the query pattern. The gap narrows:

```
$ time grep be24b574f46006147247 file
98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247

real    0m0.343s
user    0m0.227s
sys     0m0.116s
$ time rg -N be24b574f46006147247 file
98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247

real    0m0.469s
user    0m0.442s
sys     0m0.027s
```

And shorten it some more:

```
$ time grep 6147247 file
da691aa39ffdab904e33c90eb06147247bcf7974db3622b329d041893d221ba5
fc996b5d9ed0bd6d378d5f3ce2f8416d1cd498195f7b8ab4e666147247673226
79db87068e8604e00bee045390ceee761d854d00e20a4d3263f686147247a1fd
98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247

real    0m0.598s
user    0m0.513s
sys     0m0.079s
$ time rg -N 6147247 file
da691aa39ffdab904e33c90eb06147247bcf7974db3622b329d041893d221ba5
fc996b5d9ed0bd6d378d5f3ce2f8416d1cd498195f7b8ab4e666147247673226
79db87068e8604e00bee045390ceee761d854d00e20a4d3263f686147247a1fd
98be987769dd2f278f947a6f1ef588d28e0bcf43e78cbe24b574f46006147247

real    0m0.484s
user    0m0.436s
sys     0m0.047s
```

This definitely smells like Boyer Moore to me.

Thanks for the report! This bug is more properly fixed in the regex engine, so I've file a ticket: https://github.com/rust-lang/regex/issues/408 --- I'm going to close this one.

---

_Closed by @BurntSushi on 2017-10-15 21:29_

---

_Comment by @BurntSushi on 2017-12-30 21:00_

The regex crate now has an implementation of TBM, and applies to the OP's case. It will be included in the next release of ripgrep.

Note though that inputs can be crafted such that TBM won't be used, and there will be an opportunity for GNU grep to be faster. There is nothing to be done here. It is a consequence of using different literal searching algorithms that are built on heuristics.

---

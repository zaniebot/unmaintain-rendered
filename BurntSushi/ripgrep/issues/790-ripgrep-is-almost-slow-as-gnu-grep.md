```yaml
number: 790
title: ripgrep is almost slow as GNU grep
type: issue
state: closed
author: igor-raits
labels:
  - question
assignees: []
created_at: 2018-02-12T15:12:10Z
updated_at: 2018-02-12T23:18:59Z
url: https://github.com/BurntSushi/ripgrep/issues/790
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep is almost slow as GNU grep

---

_@igor-raits_

#### What version of ripgrep are you using?

```
ripgrep 0.8.0 (rev 81afe8c5a0)
+SIMD +AVX
```

#### What operating system are you using ripgrep on?

Fedora 28 (x86_64).

#### Describe your question, feature request, or bug.

```
⋊> ~/t/subtitles time env LC_ALL=C egrep -n -w 'Sherlock [A-Z]\w+' OpenSubtitles2018.raw.en >f
16.32user 3.93system 0:30.92elapsed 65%CPU (0avgtext+0avgdata 2628maxresident)k
24219232inputs+928outputs (0major+279minor)pagefaults 0swaps
⋊> ~/t/subtitles time env LC_ALL=C ~/Projects/upstream/ripgrep/target/release/rg -n -w 'Sherlock [A-Z]\w+' OpenSubtitles2018.raw.en >f
4.00user 3.12system 0:26.11elapsed 27%CPU (0avgtext+0avgdata 2016464maxresident)k
24219232inputs+952outputs (1major+295352minor)pagefaults 0swaps
```

It's really weird how ripgrep is slow and also memory consumption is weird too.

---

_Comment by @BurntSushi on 2018-02-12 18:55_

Just to ask up front, what were you expecting here? This file is 9.3GB. Judging from your page faults, it does look like it is all mostly in memory, which is good. If it wasn't, then disk bandwidth will dominate. You might try moving it to a ramdisk just to be sure.

So the comparison here requires additional explanation:

1. ripgrep does not respect system locale settings. Instead, it assumes UTF-8 (or more precisely, ASCII compatible text all the time). This means that your use of `\w` is actually different in both cases (as is the `-w`, since ripgrep's is Unicode aware and grep's is not). You can flatten out the `\w` in ripgrep to be ASCII only by using `(?-u:\w)`. However, this is a pedant point overall since the search time in this example is dominated by the `Sherlock` literal.
2. By default, ripgrep will use memory maps for a single file search. This is the case here, which is what's causing the misleading memory statistic. If you pass the `--no-mmap` flag, then the max resident size comes down to more reasonable levels. (In my case, 4-5x GNU grep, but I blame this on Rust's default jemalloc allocator, which is known to allocate a little more up front than the standard GNU system allocator.) However, ripgrep will get a little slower. :-)

Here are my results running your command, including a `--no-mmap` run, where `/tmp` is a ramdisk:

```
$ LC_ALL=C \time egrep -n -w 'Sherlock [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en | wc -l
9.43user 1.41system 0:10.85elapsed 99%CPU (0avgtext+0avgdata 2612maxresident)k
0inputs+0outputs (0major+240minor)pagefaults 0swaps
5268

$ \time rg -n -w 'Sherlock [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en | wc -l
2.37user 0.33system 0:02.70elapsed 99%CPU (0avgtext+0avgdata 9704852maxresident)k
0inputs+0outputs (0major+151654minor)pagefaults 0swaps
5268

$ \time rg -n -w 'Sherlock [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en --no-mmap | wc -l
1.89user 1.48system 0:03.37elapsed 99%CPU (0avgtext+0avgdata 10556maxresident)k
0inputs+0outputs (0major+157minor)pagefaults 0swaps
5268
```

On my end, and on my system (64GB memory, Intel i7 6900K), these times "feel" about right, although I'm surprised at how much slower GNU grep is here. My hypothesis is that ripgrep is choosing a better byte to feed to `memchr`. To test this hypothesis, we can fudge the pattern string so that GNU grep and ripgrep both use the same byte with memchr:

```
$ \time rg -n -w 'SherlockZ [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en --no-mmap | wc -l
Command exited with non-zero status 1
0.47user 1.49system 0:01.96elapsed 99%CPU (0avgtext+0avgdata 10604maxresident)k
0inputs+0outputs (0major+156minor)pagefaults 0swaps
0

$ LC_ALL=C \time egrep -n -w 'SherlockZ [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en | wc -l
Command exited with non-zero status 1
8.18user 1.30system 0:09.49elapsed 99%CPU (0avgtext+0avgdata 2804maxresident)k
0inputs+0outputs (0major+230minor)pagefaults 0swaps
0
```

ripgrep gets faster (expected, because `Z` is rarer than any byte in `Sherlock`), but GNU grep stays the same speed. This is surprising. Could the `[A-Z]\w+` be doing something weird with GNU grep literal detection? Let's remove that and check:

```
$ \time rg -n -w 'SherlockZ' /tmp/OpenSubtitles2016.raw.en --no-mmap | wc -l
Command exited with non-zero status 1
0.48user 1.47system 0:01.95elapsed 99%CPU (0avgtext+0avgdata 8532maxresident)k
0inputs+0outputs (0major+153minor)pagefaults 0swaps
0

$ LC_ALL=C \time egrep -n -w 'SherlockZ' /tmp/OpenSubtitles2016.raw.en | wc -l
Command exited with non-zero status 1
3.68user 1.29system 0:04.98elapsed 99%CPU (0avgtext+0avgdata 2636maxresident)k
0inputs+0outputs (0major+228minor)pagefaults 0swaps
0
```

Indeed! GNU grep gets twice as fast while ripgrep  stays the same. To me, this suggests there is something sub-optimal in how GNU grep is handling its literal optimizations, but I don't know what it is. As a baseline, consider GNU grep's performance with no literals:

```
$ LC_ALL=C \time egrep -n '^\w\s\w\s\w\s\w\s\w\s\w\s\w$' /tmp/OpenSubtitles2016.raw.en | wc -l
11.59user 1.28system 0:12.88elapsed 99%CPU (0avgtext+0avgdata 2684maxresident)k
0inputs+0outputs (0major+235minor)pagefaults 0swaps
86
```

This isn't that far from the query above with a literal, so perhaps something is indeed wrong with grep's literal optimizations in this case.

It's possible that line counting plays a role here. ripgrep's line counting is heavily optimized, particularly when AVX is enabled. So ripgrep doesn't slow down much, but GNU grep does:

```
$ \time rg -w 'SherlockZ [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en | wc -l
Command exited with non-zero status 1
0.75user 0.37system 0:01.13elapsed 99%CPU (0avgtext+0avgdata 9703728maxresident)k
0inputs+0outputs (0major+151651minor)pagefaults 0swaps
0

$ LC_ALL=C \time egrep -w 'SherlockZ [A-Z]\w+' /tmp/OpenSubtitles2016.raw.en | wc -l
Command exited with non-zero status 1
5.10user 1.27system 0:06.38elapsed 99%CPU (0avgtext+0avgdata 2688maxresident)k
0inputs+0outputs (0major+226minor)pagefaults 0swaps
0
```

---

_Comment by @igor-raits on 2018-02-12 19:09_

So it seems my issue was because reading from disk is slow. Once I moved file to /tmp, it started to work much faster!

Note that in system-compiled version of Rust we don't use jemalloc (as our maintainer is from toolchain team who develop glibc as well and they don't trust jemalloc :P).

I was just really surprised when reading readme which shows few seconds for grepping 9GiB file and when I tried to do same, it takes much much longer.

---

_Closed by @igor-raits on 2018-02-12 19:09_

---

_Comment by @BurntSushi on 2018-02-12 19:12_

@ignatenkobrain Ah gotya. Note that as a small errata to my above analysis, I goofed. There is a ` ` (space) in the literal, and _that's_ what GNU grep is using for its `memchr` byte, which explains why it's so slow (because ` ` is common). Using a rarer byte *properly* as the last byte shows a speed increase, which affirms that GNU grep is applying its literal optimizations:

```
$ LC_ALL=C \time egrep -n -w 'SherlockZ[A-Z]\w+' /tmp/OpenSubtitles2016.raw.en | wc -l                                                                                                      
Command exited with non-zero status 1                                                                                                                                                                                
3.39user 1.37system 0:04.77elapsed 99%CPU (0avgtext+0avgdata 2800maxresident)k                                                                                                                                       
0inputs+0outputs (0major+226minor)pagefaults 0swaps                                                                                                                                                                  
0
```

---

_Label `question` added by @BurntSushi on 2018-02-12 23:18_

---

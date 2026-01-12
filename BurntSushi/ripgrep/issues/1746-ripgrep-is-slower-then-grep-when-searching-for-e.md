```yaml
number: 1746
title: ripgrep is slower then grep when searching for e-mail regex
type: issue
state: closed
author: MaxG87
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-11-27T14:11:24Z
updated_at: 2023-11-25T20:04:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1746
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep is slower then grep when searching for e-mail regex

---

_@MaxG87_

#### What version of ripgrep are you using?

```
rg --version
ripgrep 12.1.1 (rev a6d05475fb)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime
```

#### How did you install ripgrep?

I run a locally compiled version.

#### What operating system are you using ripgrep on?

Ubuntu 20.04.

#### Describe your bug.

Searching for an e-mail pattern is faster using `grep` than `rg`. I experience this in my local Maildir and in the Linux kernels `linux/fs/` subfolder.

#### What are the steps to reproduce the behavior?

I use the Linux kernels `linux/fs` (rev 85a2c56cb4454c73f56d3099d96942e7919b292f) subfolder as an example for a folder we both have access too. In practice I would like to run the commands in my Maildir.

##### Pattern without match (~25ms vs ~135ms)
```bash
cd /path/to/linux/fs
pattern="[A-Z0-9._%+-]*lin[A-Z0-9._%+-]*tor[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
# some grep warm up runs
time grep -rhioE "$pattern" | len
0
grep --color=auto --exclude-dir={.bzr,CVS,.git,.hg,.svn,.idea,.tox} -rhioE   0,02s user 0,01s system 89% cpu 0,025 total
wc -l  0,00s user 0,00s system 4% cpu 0,024 total
# some ripgrep warm up runs
time rg -ioI "$pattern" | len
0
/tmp/rg -ioI "$pattern"  0,34s user 0,01s system 252% cpu 0,137 total
wc -l  0,00s user 0,00s system 1% cpu 0,136 total
```

##### Pattern with some matches (~50ms vs ~60ms)
```bash
cd /path/to/linux/fs
pattern="[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
# some grep warm up runs
time grep -rhioE "$pattern" | len
10
grep --color=auto --exclude-dir={.bzr,CVS,.git,.hg,.svn,.idea,.tox} -rhioE   0,02s user 0,03s system 98% cpu 0,048 total
wc -l  0,00s user 0,00s system 5% cpu 0,047 total
# some ripgrep warm up runs
time rg -ioI "$pattern" | len
10
/tmp/rg -ioI "$pattern"  0,16s user 0,03s system 294% cpu 0,065 total
wc -l  0,00s user 0,00s system 4% cpu 0,065 total
```



#### What is the expected behavior?

I would expect ripgrep to be faster than grep.


---

_Comment by @BurntSushi on 2020-11-27 15:37_

I cannot seem to reproduce this easily... But read on.

For your second example, I'd consider those times close enough (especially on such a small corpus) that they are effectively the same. But in any case, ripgrep is faster for me:

```
$ hyperfine 'rg -i "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"'
Benchmark #1: rg -i "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
  Time (mean ± σ):      19.5 ms ±   1.4 ms    [User: 115.1 ms, System: 22.6 ms]
  Range (min … max):    16.2 ms …  23.5 ms    115 runs

$ hyperfine 'grep -riE "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"'
Benchmark #1: grep -riE "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
  Time (mean ± σ):      46.3 ms ±   1.1 ms    [User: 25.3 ms, System: 20.7 ms]
  Range (min … max):    43.6 ms …  48.7 ms    58 runs
```

Now, your first example is more interesting. That's a substantial difference. On my machine, ripgrep runs a hair faster, but they're effectively tied:

```
$ hyperfine --ignore-failure 'rg -i "[A-Z0-9._%+-]*lin[A-Z0-9._%+-]*tor[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"'
Benchmark #1: rg -i "[A-Z0-9._%+-]*lin[A-Z0-9._%+-]*tor[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
  Time (mean ± σ):      40.1 ms ±   1.6 ms    [User: 364.8 ms, System: 22.6 ms]
  Range (min … max):    36.7 ms …  44.4 ms    65 runs

  Warning: Ignoring non-zero exit code.

$ hyperfine --ignore-failure 'grep -riE "[A-Z0-9._%+-]*lin[A-Z0-9._%+-]*tor[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"'
Benchmark #1: grep -riE "[A-Z0-9._%+-]*lin[A-Z0-9._%+-]*tor[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
  Time (mean ± σ):      46.8 ms ±   1.4 ms    [User: 24.0 ms, System: 22.3 ms]
  Range (min … max):    43.9 ms …  50.7 ms    59 runs

  Warning: Ignoring non-zero exit code.
```

Do you perhaps have a ripgrep config file that is running ripgrep single threaded? If I force ripgrep into a single thread, then it gets closer to your reproduction:

```
$ hyperfine 'rg -i -j1 "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"'
Benchmark #1: rg -i -j1 "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
  Time (mean ± σ):      91.7 ms ±   5.4 ms    [User: 71.1 ms, System: 20.3 ms]
  Range (min … max):    75.7 ms …  98.1 ms    29 runs
```

And if I have ripgrep search the same set of files as grep, then a real difference can be observed:

```
$ hyperfine 'rg -i -j1 -uuu "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"'
Benchmark #1: rg -i -j1 -uuu "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}"
  Time (mean ± σ):     151.0 ms ±  10.4 ms    [User: 129.2 ms, System: 21.3 ms]
  Range (min … max):   140.5 ms … 168.4 ms    17 runs
```

OK, let's see if this is a regex engine problem or a directory traversal problem. Easiest way to do that is to see if we can reproduce the performance difference on a single file. So I've just concatenated all of the files in `linux/fs` into a single file. And to make differences more pronounced (and eliminate the effects of noise), I've made the file 10x bigger:

```
$ find ./ -type f -print0 | xargs -0 cat > /tmp/linux-fs-all
$ for ((i=0;i<10;i++)); do cat /tmp/linux-fs-all; done > /tmp/linux-fs-all.10x
```

The first thing to note is that there is some binary data:

```
$ grep -iE "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /tmp/linux-fs-all.10x
grep: /tmp/linux-fs-all.10x: binary file matches
```

So let's use the `-a/--text` flag and see if we can re-create the performance difference on a single file:

```
$ time grep -aciE "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /tmp/linux-fs-all.10x
100

real    0.295
user    0.199
sys     0.096
maxmem  7 MB
faults  0

$ time rg -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /tmp/linux-fs-all.10x
100

real    1.249
user    1.231
sys     0.017
maxmem  580 MB
faults  0
```

Great, got it. Running it under `perf` shows the total time split between Teddy (a fast literal prefilter) and the regex engine. My first thought is whether this is a case where ripgrep's more aggressive literal optimizations result in it being slower than grep. To figure that out, we need to find 1) which literals ripgrep is searching for and 2) whether they have a lot of matches or not. We can do (1) by running ripgrep with the `--trace` flag:

```
$ rg -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /tmp/linux-fs-all.10x --trace
...
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:109: required literals found: [Cut(VID), Cut(vID), Cut(ViD), Cut(viD), Cut(VId), Cut(vId), Cut(Vid), Cut(vid), Cut(@), Cut(.)]
DEBUG|grep_regex::matcher|crates/regex/src/matcher.rs:50: extracted fast line regex: (?-u:VID|vID|ViD|viD|VId|vId|Vid|vid|@|\x2e)
...
```

Yeah... That doesn't look so good. Let's see how often the "fast line regex" matches:

```
$ time rg -c '(?-u:VID|vID|ViD|viD|VId|vId|Vid|vid|@|\x2e)' /tmp/linux-fs-all.10x
4489830

real    0.722
user    0.701
sys     0.020
maxmem  580 MB
faults  0
```

Yikes. That is almost certainly the problem. The killers are the `@` and `.` literals that were extracted, because they make up the vast majority of all false positives. (`0x2E` is ASCII for `.`.) If we remove them from the literal search, the number of matches drops precipitously:

```
$ time rg -c '(?-u:VID|vID|ViD|viD|VId|vId|Vid|vid)' /tmp/linux-fs-all.10x
24320

real    0.157
user    0.139
sys     0.017
maxmem  580 MB
faults  0
```

For grins, observe how ripgrep is actually faster than GNU grep on these specific queries:

```
$ time grep -cE '(?-u:VID|vID|ViD|viD|VId|vId|Vid|vid|@|\.)' /tmp/linux-fs-all.10x
4773860

real    1.098
user    0.990
sys     0.107
maxmem  7 MB
faults  0

$ time grep -cE '(?-u:VID|vID|ViD|viD|VId|vId|Vid|vid)' /tmp/linux-fs-all.10x
22320

real    0.963
user    0.895
sys     0.067
maxmem  7 MB
faults  0
```

The problem here is that ripgrep's heuristic optimization is sub-optimal in this case. These sorts of things are difficult to fix, because fixing it for one case might actually result in making other cases slower. One possible fix would be to just remove `@` and `.` from the set of literals we search. And indeed this is a correct transformation in this case. And removing things like that is probably a smart thing to do anyway, since they are very common. Well, at least `.` is. `@` is common in some corpora and not others. We can see what performance _could_ be like by replacing `@` and `.` with `\S` to defeat the literal detection:

```
$ time rg -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*\S[a-z]+\S[a-z]{2,}" /tmp/linux-fs-all.10x
10760

real    0.144
user    0.100
sys     0.043
maxmem  581 MB
faults  0
```

Obviously it doesn't produce the same results, but if you put it together into a pipeline...

```
$ time rg -ai "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*\S[a-z]+\S[a-z]{2,}" /tmp/linux-fs-all.10x | rg -aci '[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}'
100

real    0.168
user    0.134
sys     0.034
maxmem  579 MB
faults  0
```

Unfortunately, while removing `.` and `@` from the literal detection is conceptually simple, the code for extracting literals is not really flexible enough to do this easily. I've been meaning to rewrite it for years, but it's pretty thorny.

Anyway, good find and thank you for the great report!

---

_Label `bug` added by @BurntSushi on 2020-11-27 15:37_

---

_Comment by @BurntSushi on 2023-11-25 18:48_

This should be improved quite a bit in the ripgrep 14 release. It isn't still quite ideal, but better:

```
[andrew@duff fs]$ time rg -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /dev/shm/linux-fs-all.10x
110

real    0.248
user    0.227
sys     0.020
maxmem  625 MB
faults  0
[andrew@duff fs]$ time rg-13.0.0 -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /dev/shm/linux-fs-all.10x
110

real    0.662
user    0.628
sys     0.033
maxmem  624 MB
faults  0
[andrew@duff fs]$ time rg -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /dev/shm/linux-fs-all.10x --no-mmap
110

real    0.106
user    0.043
sys     0.063
maxmem  15 MB
faults  0
[andrew@duff fs]$ time rg-13.0.0 -aci "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /dev/shm/linux-fs-all.10x --no-mmap
110

real    0.678
user    0.608
sys     0.070
maxmem  15 MB
faults  0
[andrew@duff fs]$ time grep -aciE "[A-Z0-9._%+-]*vid[A-Z0-9._%+-]*@[a-z]+\.[a-z]{2,}" /dev/shm/linux-fs-all.10x
110

real    0.166
user    0.116
sys     0.050
maxmem  15 MB
faults  0
```

The interesting thing to note above is that `--no-mmap` actually runs quite a bit faster than `--mmap` in the upcoming release. Explaining this is tricky, but I'll try:

1. At time of writing, ripgrep master still tries to detect inner literals from the regex pattern (as it has always done). But, the regex engine has also grown more sophisticated inner literal detection, and ripgrep's inner literal extractor only runs if the regex engine's own detector fails. (ripgrep's is more flexible because of line oriented search.)
2. In this case, the regex engine thinks its inner literal extraction is good. Indeed, it is, and it extracts the same literals as the ripgrep's extractor. That is, `(?i:vid)`. Thus, ripgrep defers to the regex engine and doesn't try to do its own optimization. This is usually fine.
3. Even the regex engine extracts the same literals, in order for the optimization to work in the context of a general regex engine, it has to put up some safeguards to avoid worst case quadratic behavior. Those safeguards end up activating in this benchmark which forces ripgrep to, in some cases, fall back to a non-literal accelerated search.
4. The non-mmap version runs faster because ripgrep searches smaller buffers. So by a twist of fate, it spends less time in the non-accelerated regex engine than it does in the mmap version.

The main issue preventing improvement here is unfortunately abstraction boundaries. ripgrep can't really know exactly what the regex engine plans to do and how it might be sub-optimal here. The regex engine could expose something about its plan, but I'm hesitant to do such things because it is an implementation detail.

Probably the next step here is figuring out how to push ripgrep's more flexible inner literal optimization down into the regex engine so that everyone who uses the regex engine can benefit from it when doing line oriented search.

But I'm calling this good enough for now.

---

_Label `rollup` added by @BurntSushi on 2023-11-25 18:48_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---

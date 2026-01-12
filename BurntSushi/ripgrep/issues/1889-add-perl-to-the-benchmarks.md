```yaml
number: 1889
title: "Add `perl` to the benchmarks"
type: issue
state: closed
author: NightMachinery
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2021-06-11T14:16:14Z
updated_at: 2021-06-11T15:55:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1889
synced_at: 2026-01-12T16:13:24Z
```

# Add `perl` to the benchmarks

---

_@NightMachinery_

`perl` can be used to emulate `grep`, and it is, well, much more powerful than tools like `rg` (e.g., we can do the boolean patterns like `perl -lne '/x/ && !/y/ && print'`). I was wondering how their performance matches up though.

---

_Comment by @BurntSushi on 2021-06-11 14:23_

I don't know Perl personally, and I don't have any plans to learn it. In theory, if someone wanted to contribute benchmarks for it, then that would be fine. _But_, given my ignorance, it would be very difficult (or rather, time intensive) for me to be able to check that it's an appropriate apples-to-apples comparison.

So I'd say, if someone wanted to contribute Perl to the benchsuite, then that would be fine. But I think I would want a write-up to come with it that justifies why it's apples-to-apples. Even among grep tools, apples-to-apples comparisons are non-trivial and require careful scrutiny of which flags are used: https://github.com/BurntSushi/ripgrep/blob/master/benchsuite/runs/2020-10-14-archlinux-frink/raw.csv

---

_Label `enhancement` added by @BurntSushi on 2021-06-11 14:23_

---

_Label `help wanted` added by @BurntSushi on 2021-06-11 14:23_

---

_Comment by @NightMachinery on 2021-06-11 14:26_

Perhaps `ack` can be used as a stand-in for `perl`? 

Since I don't know either `perl` or `ack` all that well, I can't contribute the benchmark. Hopefully, someone who does will come around.

---

_Comment by @BurntSushi on 2021-06-11 14:31_

ack is too slow. Probably perl is too. From [my blog post](https://blog.burntsushi.net/ripgrep/):

> Notably absent from this list is ack. I chose not to benchmark it because, at the time of writing, ack was much slower than the other tools in this list. However, ack 3 is now in beta and includes some performance improvements, sometimes decreasing search times by half.

`ack` is also included in the first README benchmark, and you can see that the difference is about an order of magnitude.

If `ack` is seen as a decent approximation for the speed of `perl`, then I think this issue is satisfied until someone can show a difference with `perl` directly.

---

_Closed by @BurntSushi on 2021-06-11 14:31_

---

_Comment by @NightMachinery on 2021-06-11 14:49_

It's possible `ack` is not a good stand-in, after all. My naive test shows `perl` outperforming `ripgrep`. 

Tested on https://github.com/NightMachinary/r_HPfanfiction/tree/master/posts:
```
arr0 * | time xargs -0 perl -ne '/time.?loop\b/ && !/wasteland/ && print'

4.32s user 9.65s system 39% cpu 35.761 total; max RSS 3532
```

```
time ( command rg 'time.?loop\b' | command rg -v 'wasteland'; )  

2.21s user 13.69s system 22% cpu 1:10.11 total; max RSS 28572
```

```
arr0 * | time (xargs -0 rg 'time.?loop\b' | command rg -v 'wasteland'; )

2.53s user 13.60s system 22% cpu 1:11.19 total; max rss 8812
```

```
arr0 * | time (xargs -0 rg 'time.?loop\b' | cat)  

2.51s user 13.45s system 23% cpu 1:08.77 total; max RSS 8384
```

What is strange is that `GNU grep` is faster than `rg`, too:
```
arr0 * | time (xargs -0 ggrep -P 'time.?loop\b' | cat)  

2.83s user 5.56s system 28% cpu 29.764 total; max RSS 1752
```
---

```zsh
arr0 () { # outputs its ARGV separated by the null char
	print -nr -- "${(pj.\0.)@}"
}
```


---

_Comment by @BurntSushi on 2021-06-11 15:41_

I can't reproduce:

```
$ find ./ -type f -print0 | (time xargs -0 perl -ne '/time.?loop\b/ && !/wasteland/ && print') | wc -l

real    2.805
user    2.024
sys     0.756
maxmem  8 MB
faults  0
415
$ find ./ -type f -print0 | (time (xargs -0 rg -j1 'time.?loop\b' | rg -v 'wasteland')) | wc -l

real    1.230
user    0.523
sys     0.685
maxmem  8 MB
faults  0
415
$ find ./ -type f -print0 | (time (xargs -0 rg 'time.?loop\b' | rg -v 'wasteland')) | wc -l

real    0.880
user    0.802
sys     1.063
maxmem  8 MB
faults  0
415
```

And similarly for your commands verbatim:

```
$ arr0 * | time xargs -0 perl -ne '/time.?loop\b/ && !/wasteland/ && print' | wc -l
Can't open  100,000 word well written HP fic that made you cry at the end..8eznmp.org: No such file or directory at -e line 1, <> line 996.
Can't open  Alright bois, let's find this war veteran harry.a0eha9.org: No such file or directory at -e line 1, <> line 207375.
Can't open  FREE TODAY  Hogwarts Watches - Just pay shipping click here 10 more left...4ou0zj.org: No such file or directory at -e line 1, <> line 22426.
Can't open  Please link fics that are about Dumbledore being a mentor to Harry and stuff and teaching him actual stuff like dueling and also new spells.a4fc7p.org: No such file or directory at -e line 1, <
> line 313.
415

real    2.980
user    2.025
sys     0.660
maxmem  8 MB
faults  0

real    2.979
user    0.000
sys     0.004
maxmem  8 MB
faults  0

$ time ( command rg 'time.?loop\b' | command rg -v 'wasteland'; ) | wc -l
415

real    0.123
user    0.476
sys     0.604
maxmem  8 MB
faults  0

real    0.122
user    0.002
sys     0.000
maxmem  8 MB
faults  0
```

Some other baseline benchmarks:

```
$ find ./ -type f -print0 | (time (xargs -0 perl -ne '/ZQZQZQZQZQ/ && print')) | wc -l

real    2.671
user    1.959
sys     0.688
maxmem  8 MB
faults  0
0
$ find ./ -type f -print0 | (time (xargs -0 rg -j1 ZQZQZQZQZQ)) | wc -l

real    1.184
user    0.482
sys     0.683
maxmem  8 MB
faults  0
0
$ time rg ZQZQZQZQZQ

real    0.127
user    0.382
sys     0.592
maxmem  8 MB
faults  0
```


---

_Comment by @BurntSushi on 2021-06-11 15:43_

Is your time output saying that it's taking 30-60s to search that directory? Are you sure you aren't just benchmark disk read times?

Which version of ripgrep are you using?

---

_Comment by @NightMachinery on 2021-06-11 15:55_

@BurntSushi Yes, it takes around one minute. I have that dir on my external HDD. 

Rerunning the tests on my MBP's SSD:
```
❯ find ./ -type f -print0 | (time xargs -0 perl -ne '/time.?loop\b/ && !/wasteland/ && print') | wc -l    

xargs -0 perl -ne '/time.?loop\b/ && !/wasteland/ && print'  4.96s user 8.43s system 24% cpu 53.939 total; max RSS 3436
     415
```

```
❯ find ./ -type f -print0 | (time (xargs -0 rg -j1 'time.?loop\b' | rg -v 'wasteland')) | wc -l         
( xargs -0 rg -j1 'time.?loop\b' | rg -v 'wasteland'; )  2.32s user 9.48s system 23% cpu 50.950 total; max RSS 8920
     415
```

So you're right, it seems the benchmarks are problematic on my computer. (The SSD is not that faster, and it still takes almost a minute.)

---

```
❯ rg --version                                                                                           
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

---

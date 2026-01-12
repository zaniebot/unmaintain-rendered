```yaml
number: 33
title: benchmark time on very small corpora
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-23T19:39:25Z
updated_at: 2016-11-20T19:09:47Z
url: https://github.com/BurntSushi/ripgrep/issues/33
synced_at: 2026-01-12T18:23:11Z
```

# benchmark time on very small corpora

---

_@BurntSushi_

An [end user reports](https://lobste.rs/c/ffpppl) that `rg` isn't as fast on `ag` on very small repositories. While it seems trivial, if this is because of startup time, then it's worth investigating and fixing.


---

_Label `enhancement` added by @BurntSushi on 2016-09-23 19:39_

---

_Comment by @BurntSushi on 2016-09-25 14:07_

#77 is another report of `grep` beating `rg` on a single file because of startup time.


---

_Comment by @BurntSushi on 2016-09-26 11:36_

Someone [ran micobenchmarks](https://www.reddit.com/r/rust/comments/544hnk/ripgrep_is_faster_than_grep_ag_git_grep_ucg_pt/d82ch6i) on infinitesimally sized input. `grep` is twice as fast.


---

_Comment by @BurntSushi on 2016-11-20 19:09_

OK, I think I can call this one done. I think a variety of small improvements have mostly fixed this:
- Switch from Docopt to clap (probably the biggest contributor).
- Permit ignore handling/parsing to reuse previous results when applicable.
- Use a parallel recursive directory iterator.

In particular, on the previously linked microbenchmark:

```
$ python -m timeit -n 1 -r 10 -v -s 'import os' 'os.system("cat /etc/group | grep 1000 > /dev/null")'
raw times: 0.0115 0.0104 0.0103 0.0104 0.00902 0.00929 0.00842 0.00919 0.00792 0.00861
1 loops, best of 10: 7.92 msec per loop
$ python -m timeit -n 1 -r 10 -v -s 'import os' 'os.system("cat /etc/group | rg 1000 > /dev/null")'
raw times: 0.0103 0.0103 0.00946 0.00881 0.00871 0.00809 0.00802 0.00764 0.00722 0.00714
1 loops, best of 10: 7.14 msec per loop
```

Compare this with ripgrep 0.2.6:

```
python -m timeit -n 1 -r 10 -v -s 'import os' 'os.system("cat /etc/group | rg-0.2.6 1000 > /dev/null")'
raw times: 0.0261 0.0251 0.0257 0.0217 0.0234 0.0239 0.0237 0.0235 0.022 0.0245
1 loops, best of 10: 21.7 msec per loop
```

I've also done some testing on small repos. This repo qualifies as quite small. There is a _ton_ of variance. The following were ran in succession:

```
[andrew@Cheetah ripgrep] time rg ripgrep | wc -l
305

real    0m0.033s
user    0m0.257s
sys     0m0.007s
[andrew@Cheetah ripgrep] time rg ripgrep | wc -l
305

real    0m0.025s
user    0m0.157s
sys     0m0.023s
[andrew@Cheetah ripgrep] time rg ripgrep | wc -l
305

real    0m0.013s
user    0m0.017s
sys     0m0.030s
[andrew@Cheetah ripgrep] time rg ripgrep | wc -l
305

real    0m0.040s
user    0m0.270s
sys     0m0.007s
```

`ag` seems to have slightly less variance:

```
[andrew@Cheetah ripgrep] time ag ripgrep | wc -l
306

real    0m0.029s
user    0m0.007s
sys     0m0.010s
[andrew@Cheetah ripgrep] time ag ripgrep | wc -l
306

real    0m0.022s
user    0m0.007s
sys     0m0.010s
[andrew@Cheetah ripgrep] time ag ripgrep | wc -l
306

real    0m0.023s
user    0m0.020s
sys     0m0.003s
[andrew@Cheetah ripgrep] time ag ripgrep | wc -l
306

real    0m0.016s
user    0m0.013s
sys     0m0.003s
```

But, if we fix both programs to use a single thread, then variance becomes almost non-existent:

```
[andrew@Cheetah ripgrep] time rg -j1 ripgrep | wc -l
305

real    0m0.011s
user    0m0.003s
sys     0m0.007s
[andrew@Cheetah ripgrep] time rg -j1 ripgrep | wc -l
305

real    0m0.011s
user    0m0.007s
sys     0m0.007s
[andrew@Cheetah ripgrep] time rg -j1 ripgrep | wc -l
305

real    0m0.011s
user    0m0.000s
sys     0m0.010s
[andrew@Cheetah ripgrep] time rg -j1 ripgrep | wc -l
305

real    0m0.010s
user    0m0.003s
sys     0m0.003s
```

and `ag`:

```
[andrew@Cheetah ripgrep] time ag --workers 1 ripgrep | wc -l
306

real    0m0.018s
user    0m0.010s
sys     0m0.000s
[andrew@Cheetah ripgrep] time ag --workers 1 ripgrep | wc -l
306

real    0m0.018s
user    0m0.010s
sys     0m0.000s
[andrew@Cheetah ripgrep] time ag --workers 1 ripgrep | wc -l
306

real    0m0.018s
user    0m0.010s
sys     0m0.003s
[andrew@Cheetah ripgrep] time ag --workers 1 ripgrep | wc -l
306

real    0m0.021s
user    0m0.007s
sys     0m0.003s
```

In other words, on small corpora, ripgrep (and, to a lesser extent, `ag`) seem to be quite susceptible to the overhead of creating threads, which also seems to introduce large variance.

I'm not sure there's anything we can really do about it. The difference in this particular example, at least, is nominal. From a user's perspective, we don't really care about the difference between 10ms and 30ms. However, the place where this can hurt us is if folks use ripgrep like it was grep, for example, in an `xargs` pipeline:

```
$ time find ./ -name '*.[ch]' -print0 | xargs -0 -P8 grep PM_RESUME | wc -l
10

real    0m0.213s
user    0m0.423s
sys     0m0.330s
$ time find ./ -name '*.[ch]' -print0 | xargs -0 -P8 rg PM_RESUME | wc -l
10

real    0m0.402s
user    0m4.750s
sys     0m0.460s
```

The overhead of ripgrep launching threads ends up making it twice as slow. Of course, in this case, ripgrep launching threads doesn't really make much sense since `xargs` is doing it for us. But this isn't something that a user will _obviously_ know. Of course, forcing ripgrep to use a single thread brings it back down to grep speeds:

```
time find ./ -name '*.[ch]' -print0 | xargs -0 -P8 rg -j1 PM_RESUME | wc -l
10

real    0m0.206s
user    0m0.507s
sys     0m0.487s
```

The other case to consider is using `xargs` without `-P` and having ripgrep handle parallelism:

```
time find ./ -name '*.[ch]' -print0 | xargs -0 rg PM_RESUME | wc -l
10

real    0m0.463s
user    0m1.297s
sys     0m0.597s
```

While worse than using `xargs -P`, compare with grep:

```
time find ./ -name '*.[ch]' -print0 | xargs -0 grep PM_RESUME | wc -l
10

real    0m0.655s
user    0m0.477s
sys     0m0.347s
```

I'm not sure there's really too much we can do here. An end user _can_ use ripgrep in an `xargs` pipeline optimally, but it definitely requires a bit of intuition on the user's behalf to realize that ripgrep is already multithreaded where as grep is not.

(This is yet another example proving that benchmarking these tools is ridiculously hard.)


---

_Closed by @BurntSushi on 2016-11-20 19:09_

---

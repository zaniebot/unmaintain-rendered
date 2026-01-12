```yaml
number: 141
title: avoid using an Arc for quiet_matched
type: pull_request
state: closed
author: lilydjwg
labels: []
assignees: []
base: master
head: master
created_at: 2016-10-01T09:49:08Z
updated_at: 2016-10-01T13:25:04Z
url: https://github.com/BurntSushi/ripgrep/pull/141
synced_at: 2026-01-12T18:23:12Z
```

# avoid using an Arc for quiet_matched

---

_@lilydjwg_

I'm sorry, the result isn't stable, and I can't tell whether it's faster or slower....


---

_Closed by @lilydjwg on 2016-10-01 09:58_

---

_Comment by @BurntSushi on 2016-10-01 11:41_

Yeah, I wouldn't expect this to make a difference. An Arc should only impose overhead when it's cloned, not when it's accessed. We only clone it once for each worker, which means the overhead is likely imperceptible.

Can you explain more about your benchmark?


---

_Comment by @lilydjwg on 2016-10-01 11:58_

I just use perf to see where it takes the most time.

```
sudo perf record -F 99 -g -- rg xxx >/dev/null
```

then

```
sudo perf report
```

and press <kbd>a</kbd> on the function that uses the most of time to see assembly. Not very easy to read, but there is a `mfence` instruction, which clearly corresponds to the `fence(SeqCst)` call inside the `steal()` function.

![a](https://cloud.githubusercontent.com/assets/440661/19013920/5e59f27e-8811-11e6-88a0-6f4415ae8674.png)

Today rg took more than one minute to complete a search, so I tried to find out why. The time reduced to several seconds when I built the new master.


---

_Comment by @BurntSushi on 2016-10-01 12:03_

Profiles can be hard to read, and in this case, it is probably misleading you. The profile reveals a lot of time spent in the work queue because it is _spinning_ waiting for work. That is, the directory iterator can't produce work fast enough for the search workers.

The time reduction on master almost certainly means that the directory you're searching has a very large gitignore file. I implemented a crude stopgap measure to make this faster, but I'm still working on it.

If I'm right, then you should notice a large improvement by running with the `-u` flag.

One way to extract a less confusing profile is to pass `-j1`. This will cause single threaded search to, which doesn't use a work queue at all.


---

_Comment by @lilydjwg on 2016-10-01 12:59_

So there is a spinning lock? It makes my fan spin loudly....I prefer it to block instead.

There is no gitignore file, though it's in a (huge) git repo---the "community" repo from Arch Linux. `-u` indeed makes it much faster.

I take the source code outside the repo, and it's slow the same.

With `-j1` rg uses only one core and it takes less time! Though it's still  much slower than `ag --workers 1`. And with `-j1`, the CPU time is spent on `regex::re_bytes::Regex::shortest_match_at` and `memchr` and `regex_syntax::Expr::simplify::simp`.


---

_Comment by @BurntSushi on 2016-10-01 13:04_

Could you please tell me how you're running your test? What command did you run to get the data? What `rg` commands are you running?

> So there is a spinning lock? It makes my fan spin loudly....I prefer it to block instead.

This is how lock free queues work. We should fix the producer.


---

_Comment by @lilydjwg on 2016-10-01 13:07_

I run this in the `percona-server-5.7.14-8` directory

```
time ./rg utf8mb4 >/dev/null
```

> This is how lock free queues work. We should fix the producer.

Does this mean that, if I search on slow disks, it takes a lot of CPU time to wait?


---

_Comment by @BurntSushi on 2016-10-01 13:11_

Could you please tell me how to get the data you're searching? What command should I run?

> Does this mean that, if I search on slow disks, it takes a lot of CPU time to wait?

No, it doesn't mean that. If the disks are slow and the corpora isn't memory, then the search workers will also be slower.


---

_Comment by @lilydjwg on 2016-10-01 13:14_

Extract [this tarball](http://www.percona.com/downloads/Percona-Server-5.7/Percona-Server-5.7.14-8/source/tarball/percona-server-5.7.14-8.tar.gz).

> No, it doesn't mean that. If the disks are slow and the corpora isn't memory, then the search workers will also be slower.

I see, thanks for the explanation :-)


---

_Comment by @BurntSushi on 2016-10-01 13:16_

> There is no gitignore file, though it's in a (huge) git repo---the "community" repo from Arch Linux. -u indeed makes it much faster.

Could you show me how to get this data as well? And the `rg` command you're running?

(I'm asking because it looks like there's more going on here. The only difference between `rg PAT` and `rg -u PAT` is that the latter doesn't look at ignore files.)


---

_Comment by @BurntSushi on 2016-10-01 13:18_

For `percona-server`, it seems clear to me that you're running into #134. There is a _giant_ `.gitignore` file in the root directory of that tarball. This is where `rg` is spending all of its time. Since directory traversal is single threaded, it can't keep up with the search workers, so the search workers are constantly asking for more work, causing the queue to spin and use up CPU.


---

_Comment by @lilydjwg on 2016-10-01 13:19_

Oh, I see! I used `ag -g` to search for `gitignore`, but it didn't search for hidden files either!


---

_Comment by @BurntSushi on 2016-10-01 13:22_

Indeed. On `percona-server`, I get these timings, with my current code (which has a few more optimizations than `master` does):

```
[andrew@Cheetah percona-server-5.7.14-8] time rg utf8mb4 | wc -l
3114

real    0m0.813s
user    0m6.163s
sys     0m0.183s
[andrew@Cheetah percona-server-5.7.14-8] time ag utf8mb4 | wc -l
3210

real    0m1.687s
user    0m2.263s
sys     0m0.930s
```

(The counts are off because I'm pretty sure `ag` is getting the `.gitignore` file wrong in some places.)

If we ask both tools to skip ignore files, then they both get a lot faster, but `rg` still wins:

```
[andrew@Cheetah percona-server-5.7.14-8] time rg -u utf8mb4 | wc -l
3117

real    0m0.118s
user    0m0.323s
sys     0m0.350s
[andrew@Cheetah percona-server-5.7.14-8] time ag -u utf8mb4 | wc -l
3214

real    0m0.399s
user    0m0.617s
sys     0m1.053s
```

The difference is almost entirely attributable to `ag`'s use of memory maps. Watch what happens when we ask `rg` to use memory maps:

```
[andrew@Cheetah percona-server-5.7.14-8] time rg -u --mmap utf8mb4 | wc -l
3117

real    0m0.354s
user    0m0.347s
sys     0m0.763s
```


---

_Comment by @BurntSushi on 2016-10-01 13:25_

@lilydjwg FYI, I made a few (important) edits to my previous comment.


---

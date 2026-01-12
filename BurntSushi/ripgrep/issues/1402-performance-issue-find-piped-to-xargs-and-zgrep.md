```yaml
number: 1402
title: Performance issue, find piped to xargs and zgrep is 15-20% faster on directories of compressed log files
type: issue
state: closed
author: rbnor
labels:
  - invalid
assignees: []
created_at: 2019-10-11T07:51:57Z
updated_at: 2020-03-15T17:28:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1402
synced_at: 2026-01-12T16:13:23Z
```

# Performance issue, find piped to xargs and zgrep is 15-20% faster on directories of compressed log files

---

_@rbnor_

#### What version of ripgrep are you using?
ripgrep 0.9.0 -SIMD -AVX
EDIT: Updated to the latest version, lost another 15 seconds on the same search.
#### How did you install ripgrep?
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/11.0.2/ripgrep_11.0.2_amd64.deb
 sudo dpkg -i ripgrep_11.0.2_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04.6 LTS

#### Describe your question, feature request, or bug.
Searching through directories of compressed log files (.gz) its faster to use find , xargs, zgrep than it is to use rg. 

#### If this is a bug, what are the steps to reproduce the behavior?
time rg -z -Ff test /aaa/bbb/ccc/ddd/2019/2019-0[1,2]* > test.res
real    1m42.605s
user    6m10.424s
sys     9m11.948s

time find /aaa/bbb/ccc/ddd/2019/2019-0[1,2]* -name "*" -print0 |xargs -P16 -0 zgrep -Ff test > test.res

real    0m58.073s
user    4m52.100s
sys     1m46.900s

Where test contains three newline separated IP-addresses, speccifying the nymber of threads for rg only lead to a marginal improvement, a matter of a couple of seconds. 

#### If this is a bug, what is the actual behavior?

Well,  it searches, completes the search with the correct results, but its just... slower. 

#### If this is a bug, what is the expected behavior?

I would expect rg to be faster


---

_Comment by @BurntSushi on 2019-10-11 11:58_

Thanks for the report, but this isn't really actionable without a corpus for me to reproduce this on. Otherwise, I can only guess: your directory contains many small files and the performance overhead of spinning up a gzip process for each file is impacting it. Another guess is that your log files are so big that you are just measuring disk read speeds, which might be highly variable unless you're doing a very careful benchmark.

But really, performance bugs need to come with a reproduction.

---

_Comment by @BurntSushi on 2019-10-11 12:00_

Please also consider trying to build master from source and trying that.

---

_Comment by @rbnor on 2019-10-11 12:12_

I suspect its unrelated to rg, somehow. Because testing with just one day its almost 8 times as fast as zgrep. 

rg -zFf test /aaa/bbb/ccc/ddd/2019/2019-01-18/*

On one month worth of logs specified as 2019-01* rg is 35% faster. , and two months add upp, zgrep still uses around 58 seconds, for one month AND for two months. 




---

_Comment by @BurntSushi on 2019-10-11 12:14_

It's possible you are hitting a performance bug that was fixed on master and is not in any release. Can you try building ripgrep from source?

---

_Comment by @rbnor on 2019-10-11 12:18_

Will try that now, thanks. 

Wish i could hand over the logs but i cant, and i know filing a report like this without the data is kind of stupid, will try to create the same kind of data elsewhere to reproduce if building from master doesnt work. 

---

_Comment by @rbnor on 2019-10-11 12:59_

cloned the repo and built with cargo, would that be revision ripgrep 11.0.2 rev 8892bf648c or? Still takes about the same time. What i dont understand is how zgrep can spend the same time on one and two months whereas rg spends double the time for two months, as expected, , which leads me to think that zgrep is only doing half the work it should. The time doesnt show increased palatalization, and neither does the perf stats -d command. 



---

_Comment by @BurntSushi on 2019-10-11 13:16_

Yeah, that's right. Unfortunately, at this point, I think the only path forward is a reproduction so that I can investigate. (Unless you yourself wanted to dig into it.) You could try finding a small subset of your data on which you can reproduce the problem, and then potentially redact it perhaps by replacing every non-whitespace character with an `x` or something. (And hopefully redacting it doesn't change the performance characteristics.)

---

_Comment by @rbnor on 2019-10-11 17:10_

I will try to do that Monday. Before I left work I did some more testing and checked the results find xargs zgrep would find vs rg, and it does find all the same results. I believe the first method, from what I can see is way better at parallelizing the opening of the files to be searched in/ the directory traversal. Running it with -l it was clear that it was checking both months at the same time at a higher rate, but I will formalize the testing. It’s interesting tho that twice the data for the first method doesn’t mean twice the time, meaning parallelisation of the io(since system time isn’t higher) whereas rg doubles in time with double the data/directories(not sure who matters the most.) 

Will get you some data to reproduce it, and run more in depth tests. 

---

_Comment by @BurntSushi on 2019-10-11 17:22_

Thanks! Appreciate it!

---

_Comment by @rbnor on 2019-10-12 18:56_

I think i was able to create some relevant test data, a bit limited on time, but the content shouldnt matter much as i were able to reproduce. 

https://github.com/RuneBergh/example_data/blob/master/testdir.tar.gz , in case it wasnt obvious i ran my tests on the archives zipped here( with tons of zipped "log files" inside them), so one needs to unzip it first. 

Was only able to do some very brief testing on this data, on my x1 carbon, but the results are similar to what i exparienced before. 

time find * -name '*' -print0 | xargs -P8 -0 zgrep -Fl 10.0.0.1 | wc -l
vs
time rg -zlF 10.0.0.1 | wc -l

rg spent 3-4 times as long time here, and the first method clearly showed way better CPU utilization, 100% across all 8 cores whereas rg just doesnt use the resources available in a lot of cases.  Keep in mind that im only using all my cores here because IO is fast enough, so it would of course have to be adabted and tested with both all and half available cores. Looking at how many processes rg uses tends to be a good indication. You are probably aware of this, just noting it in case others are testing as well. 

So where does the limit go? 
Numbers are aproximates, but tested. 
Searching 1 day rg is 3x faster
Searching 2 days rg is 3x faster
Searching 4 days rg is 3x faster
Searching 8 days rg is 1.3x faster
Searching 16 days rg is ( didnt test this yet)
Searching 30 days rg is 0.5x slower
Searching 60 days rg is 0.25x slower ( and crashes half way through, only showing half the expected line count, so most likely even slower)




---

_Comment by @rbnor on 2019-10-13 09:07_

This is almost as fast as zgrep:
time find 2019-01-*/* -name '*' -print0 | xargs -P8 -0 -I {} sh -c 'zcat {} |rg -Fl 10.0.0.1' | wc -l

Still takes 30% longer than zgrep, but its closer, and faster than rg on its own(which is another 20% slower).  The way i see it the problem is somewhat narrowed down to the parallelism of how rg works with a large number of files, since zcat invokes gzip -cd that art really shouldnt matter, but what i have learned is that what "should be" doesnt matter, benchmarks does.  Parallel takes twice as long as the same xargs version if that matters. 

I mean htop says it all, it just doesnt use the power thats made availabe, at all, it seems to suffer from some of the same performance issues as parallel for this kind of usecase. 

So this is still the fastest way: 
time find 2019-01-*/* -name '*' -print0 | xargs -P8 -0 zgrep -Fl 10.0.0.1 | wc -l

But i dont want to stick with this because its "good enought" i want the performance increase of rg on larger directories as well, as its insanely fast on a smaller scale.

---

_Comment by @rbnor on 2019-10-20 18:34_

@BurntSushi Were you able to replicate this ? 

---

_Comment by @BurntSushi on 2019-10-20 19:59_

I haven't had time to look into this. I will post an update when I do.

On Sun, Oct 20, 2019, 14:34 RuneBergh <notifications@github.com> wrote:

> @BurntSushi <https://github.com/BurntSushi> Were you able to replicate
> this ?
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1402?email_source=notifications&email_token=AADPPYVRS4SE5NA7MIN3I3TQPSQD7A5CNFSM4I7WKAW2YY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGOEBYQVMA#issuecomment-544279216>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADPPYWQCV335C6PKGODLUTQPSQD7ANCNFSM4I7WKAWQ>
> .
>


---

_Comment by @rbnor on 2019-10-28 18:53_

cool


---

_Comment by @BurntSushi on 2020-03-15 16:32_

I can't reproduce this on ripgrep master:

```
$ time find ./2019-02-9 -type f -print0 | xargs -n20 -P16 -0 zgrep -Fl 10.0.0.1 | wc -l
1000

real    6.075
user    1:05.71
sys     8.164
maxmem  5 MB
faults  0

$ time rg -zlF 10.0.0.1 2019-02-9 | wc -l
1000

real    0.187
user    0.061
sys     0.129
maxmem  7 MB
faults  0
```

I can't reproduce this on ripgrep 11.0.2 either:

```
$ time rg-11.0.2-pcre2 -zlF 10.0.0.1 2019-02-9 | wc -l
1000

real    0.184
user    0.040
sys     0.156
maxmem  7 MB
faults  0
```

Overall, I found your comments here pretty hard to follow. Your `find | xargs` commands were also not quite right (see my tweaked command above).

---

_Closed by @BurntSushi on 2020-03-15 16:32_

---

_Comment by @BurntSushi on 2020-03-15 16:35_

If you can provide a better reproduction, I'd be happy to look into it. When providing a reproduction, please tell me _exactly_ which commands I should be running. It should start with getting the data.

---

_Label `invalid` added by @BurntSushi on 2020-03-15 16:35_

---

_Comment by @rbnor on 2020-03-15 17:28_

Never mind, i switched to ugrep long time ago, its faster anyways.

On Sun, 15 Mar 2020 at 17:35, Andrew Gallant <notifications@github.com>
wrote:

> If you can provide a better reproduction, I'd be happy to look into it.
> When providing a reproduction, please tell me *exactly* which commands I
> should be running. It should start with getting the data.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1402#issuecomment-599233708>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AGJIWV254GYZ7Y6S422FF4TRHT7U5ANCNFSM4I7WKAWQ>
> .
>


---

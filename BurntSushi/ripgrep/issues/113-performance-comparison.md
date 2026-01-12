```yaml
number: 113
title: Performance comparison
type: issue
state: closed
author: floyd-fuh
labels: []
assignees: []
created_at: 2016-09-27T07:17:40Z
updated_at: 2017-01-27T20:29:42Z
url: https://github.com/BurntSushi/ripgrep/issues/113
synced_at: 2026-01-12T16:13:21Z
```

# Performance comparison

---

_@floyd-fuh_

This is just a fyi with a couple of questions, feel free to close the issue if you think this doesn't fit here.

I tried running the grep-it.sh script from my crass project with ripgrep:

https://github.com/floyd-fuh/crass/blob/master/grep-it.sh

But it doesn't seem to be much faster on data that has only few matches:

https://github.com/floyd-fuh/crass/blob/123ac1adb81c3a2e05086824d2532ff4bed0ce7c/testing/ripgrep-test.txt

The regex are very specific and therefore there are usually only very few matches at all. I only used the "time" command for benchmarking but ripgrep wasn't really that much faster. ripgrep failed to interprete two regex (one look ahead and one with the command line argument -o) and therefore even did two searches less than gnu grep. The biggest benefit was that ripgrep used 4 cores instead of only 1. As soons as I activated the BACKGROUND and switched MAX_PROCESSES to 4, gnu grep and ripgrep used roughly the same time, although in the BACKGROUND case the script only checks every 0.25 seconds (sleep 0.25) if a new subprocess with gnu grep can be scheduled.

It would be interesting to hear why you think it didn't outperform gnu grep.

Currently I don't think I'll support ripgrep in the project, because a) it is not really faster in that usecase, b) it doesn't support all the regex and c) has to be installed manually while gnu grep is usually already there.


---

_Comment by @BurntSushi on 2016-09-27 13:00_

I'm not really sure how to analyze what you've given me. It's not clear at all what you're actually timing, what you're searching or how to actually reproduce what you've done. I need at least that much to give you a meaningful response. If your benchmark is taking over 2 minutes to run, then I expect that to correspond to a _tremendous_ amount of data or a ton of searches or both. Is that true? That's a really important fact because if it is a tremendous amount of data, then you're likely measuring disk I/O, not search speed.

> It would be interesting to hear why you think it didn't outperform gnu grep.

I couldn't even possibly begin to answer that question. Did you by chance happen to [read my blog post](http://blog.burntsushi.net/ripgrep/) on the topic? I get that it's long, but that's because **benchmarks are hard**. I need to understand much more precisely what you're doing.

For example, if your corpus is a bunch of really really tiny files and your searches are simple literals, then I'd guess both programs are spending all their time in directory iteration, and will therefore have comparable times.

> it is not really faster in that usecase

**`ripgrep` isn't faster than `grep` in every use case.** For example, if you're searching a bunch of files on disk that aren't actually in memory, then `rg` and `grep` are likely going to perform similarly because the limiting factor is actually reading from disk. `rg`, in all likelihood, _can't_ be faster than `grep` in that case.


---

_Comment by @BurntSushi on 2016-09-27 13:03_

> ripgrep failed to interprete two regex (one look ahead and one with the command line argument -o)

`ripgrep` is based on finite automata and therefore doesn't support look around. `egrep` doesn't either. Only `grep -P` does.

The `-o/--only-matching` flag is on the list of things to add. :-)


---

_Comment by @floyd-fuh on 2016-09-27 13:24_

I totally get that benchmarking is hard. I didn't try to do benchmarking. That's not a benchmark :) . It's a single use case. And you are might be right, maybe I am just measuring disc speed (IO). The disc is an SSD. I didn't think about what is actually the bottleneck, I just ran a comparison for my use case. Again I wasn't trying to prove/disprove anything. I just ran a test and thought you might be interested. Yes I read your blog post. And the result of my tests was that it doesn't matter what I use (gnu grep -P or ripgrep) for my project.

Yes, it's a tremendous amount of data if you want to call it that (7 Android apps run through apktool and dex2jar and not cleaning up, therefore leaving everything in that folder such as zip files etc.).

I was just really surprised that the difference was so small. Unfortunately I can't release my test data set. If you want you can do your own tests with grep-it.sh with some public data.

Maybe it's helpful if you include in your blog post that disc IO could be a bottleneck rather then the grep software


---

_Closed by @floyd-fuh on 2016-09-27 13:24_

---

_Comment by @BurntSushi on 2016-09-27 13:30_

> I was just really surprised that the difference was so small.

If you take a look at my benchmarks in my blog, you can see how the tools are much closer on some benchmarks but much farther apart on others. For example, on this one, a correct `grep` search takes over **4 minutes**, but `rg` only takes a few seconds: http://blog.burntsushi.net/ripgrep/#subtitles-no-literal --- But this is actually benchmarking the software itself, not the rate at which your disk can offer up data.

> Maybe it's helpful if you include in your blog post that disc IO could be a bottleneck rather then the grep software

I did. Read the methodology section. :-) Specifically:

> Every benchmarked command is run three times before being measured as a “warm up.” Specifically, this is to ensure that the corpora being searched is already in the operating system’s page cache. If we didn’t do this, we might end up benchmarking disk I/O, which is not only uninteresting for our purposes, but is probably not a common end user scenario. It’s more likely that you’ll be executing lots of searches against the same corpus (at least, I know I do).


---

_Comment by @victoriastuart on 2017-01-27 19:01_

Hi: re benchmarking, I just ran some tests on my system [Arch Linux x86_64; note: SSD (~70+ times faster than my HDD; xfce4; ...]:

COMPARISON:

    $ pwd
        /home/victoria

    $ ls 2>/dev/null -laR | wc -l
        1951389    ## << ~1.95M files

* "Static Build Tools:"  is line 12 in /home/victoria/projects/shortcuts/ut-x
* Compare grep vs rg (ripgrep)
* ~/.bashrc:
  * alias grep='grep 2>/dev/null --color=always'
  * alias rg='rg --color always'

**1. grep:**

    $ time grep -r . -e "Static Build Tools:"
        ./projects/shortcuts/ut-x:Static Build Tools:
        Command exited with non-zero status 2
        10:04.24

* 10 min 04 sec!!
* Drilling down (to make grep search easier):

    $ cd projects/

* repeat search:

    $ time grep -r . -e "Static Build Tools:"
        ./shortcuts/ut-x:Static Build Tools:
        0:43.19

* ... 43 sec

    $ cd shortcuts/
    $ time grep -r . -e "Static Build Tools:"
        ./ut-x:Static Build Tools:
        0:00.00

    $ cd ~
    $ pwd
        /home/victoria

**2. ripgrep (rg):**

    $ time rg . -e "Static Build Tools:"
        projects/shortcuts/ut-x
        12:Static Build Tools:
        1:22.83

* 1 min 23 sec (ripgrep), vs. 10 min 04 sec (grep)

    $ cd projects/
    
    $ time rg . -e "Static Build Tools:"
        shortcuts/ut-x
        12:Static Build Tools:
        0:00.36

* <1 sec (rg) vs. 43 sec (grep)!

    $ cd shortcuts/

    $ time rg . -e "Static Build Tools:"
        ut-x
        12:Static Build Tools:
        0:00.01    ## << i.e. instantaneous! (faster than 'time' can report, essentially!  ;-)

* Q.E.D.! :-D


---

_Comment by @BurntSushi on 2017-01-27 20:12_

@victoriastuart Nice results. :-) I'm willing to bet you've got some big binary files in that directory that ripgrep is skipping but that grep isn't. :-)

---

_Comment by @victoriastuart on 2017-01-27 20:23_

lol, maybe/probably :-p

Those tests (just a quick 'hack') were done on a 500GB SSD (/home/...), that I use for my programming work (Python; ML; ...). There are a lot of data files, virtual environments, ... etc. on that fast SSD.

/ [root] is on another/dedicated (250 GB) SSD.

I keep GitHub repos, scripts, other stuff on a 5 TB HDD ... all backed up (rsync/rsnapshot: 4x daily) to another 5 TB HDD.

Anyway, "as is" ripgrep appears to be blazing fast;  I only became aware of it a couple of days ago, from this great article,

https://medium.com/@crashybang/supercharge-vim-with-fzf-and-ripgrep-d4661fc853d2#.57jipcwcw

... so I decided to check it out.  Nice work; thank you!!  :-D

---

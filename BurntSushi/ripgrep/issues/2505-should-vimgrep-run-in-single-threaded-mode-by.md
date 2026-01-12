```yaml
number: 2505
title: should --vimgrep run in single threaded mode by default?
type: issue
state: closed
author: voidus
labels:
  - question
  - rollup
assignees: []
created_at: 2023-05-05T14:01:49Z
updated_at: 2023-11-25T20:03:58Z
url: https://github.com/BurntSushi/ripgrep/issues/2505
synced_at: 2026-01-12T16:13:24Z
```

# should --vimgrep run in single threaded mode by default?

---

_@voidus_

I think I'm hitting #999 again

#### What version of ripgrep are you using?

`ripgrep 13.0.0` 

#### How did you install ripgrep?

Via nix' home-manager @nixpkgs rev 402cc3633cc60dfc50378197305c984518b30773

#### What operating system are you using ripgrep on?

Arch linux with a lot of stuff (including rg) coming from nix

#### Describe your bug.

Project info:

```> du -sh .; echo "incl gitignore"; find -type f | wc -l; find -type f -print0 | xargs -0 cat | wc -l; echo "excl gitignore"; fd --type f | wc -l; fd --type f --print0 | xargs -0 cat | wc -l;  
98M	.
incl gitignore
5717
90682
excl gitignore
410
27827

 > nix run nixpkgs#time -- -v rg a | wc -l        
	Command being timed: "rg a"
	User time (seconds): 0.00
	System time (seconds): 0.00
	Percent of CPU this job got: 160%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.01
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 10880
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 0
	Minor (reclaiming a frame) page faults: 2214
	Voluntary context switches: 211
	Involuntary context switches: 29
	Swaps: 0
	File system inputs: 0
	File system outputs: 0
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0
14008

> nix run nixpkgs#time -- -v rg --vimgrep a | wc -l
Command terminated by signal 9
	Command being timed: "rg --vimgrep a"
	User time (seconds): 4.67
	System time (seconds): 35.86
	Percent of CPU this job got: 85%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:47.30
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 26359456
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 15488
	Minor (reclaiming a frame) page faults: 7088456
	Voluntary context switches: 467064
	Involuntary context switches: 52122
	Swaps: 0
	File system inputs: 1722872
	File system outputs: 0
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0
89447
```

26Gigs of memory to search through a hundred megs of stuff seems like an issue.

I `^c`'d the vimgrep one once my 32GB of memory + 18GB of swap were full.

I'm running this in this subfolder: https://gitlab.com/sea-watch.org/planner/-/tree/main/backoffice but I'm working in this repo so it might not be the same.
I did move the big .direnv folder with the python virtualenvs out of the way to run the above test.

I'd be more than happy to debug this further if you have any guidance.

---

_Comment by @BurntSushi on 2023-05-05 14:18_

This is just #999 but with different data. The walkthrough I did in that issue is precisely relevant to your case.

First, let's capture all of the results (in my case, `--vimgrep` uses less memory than it does for you, but it's still an exorbitant amount):

```
$ time rg a > /tmp/rg2505-normal.txt

real    0.020
user    0.027
sys     0.018
maxmem  13 MB
faults  0

$ time rg a --vimgrep > /tmp/rg2505-vimgrep.txt

real    6.090
user    1.063
sys     8.381
maxmem  18370 MB
faults  0
```

OK, that seems crazy, but how big is the actual output?

```
$ ls -lh /tmp/rg2505-*.txt
-rw-rw-r-- 1 andrew users 2.7M May  5 10:10 /tmp/rg2505-normal.txt
-rw-rw-r-- 1 andrew users  18G May  5 10:10 /tmp/rg2505-vimgrep.txt
```

Well, okay, there's your problem. Case closed. ripgrep is using a lot of memory because you've asked it to. Note the docs of `--vimgrep`:

```
--vimgrep                                
    Show results with every match on its own line, including line numbers and
    column numbers. With this option, a line with more than one match will be
    printed more than once.
```

That last sentence is telling you what's happening here: for every match ripgrep finds, the entire line is printed. This is presumably what editors like `vim` expect.

This means that even if your corpus is very small, the output as a result of `--vimgrep` can still be extremely large. Indeed, the relationship is worst case quadratic. You can compute the worst case concretely by multiplying the total size of the corpus (4.5 MB in this case it looks like) by the total number of bytes in the corpus (4,500,000). That gives us about 20 GB here, which is a bit more than what ripgrep actually used. It's not a precise bound, but it's pretty close.


---

_Closed by @BurntSushi on 2023-05-05 14:18_

---

_Label `duplicate` added by @BurntSushi on 2023-05-05 14:18_

---

_Comment by @voidus on 2023-05-05 15:09_

Yes, that makes sense. Sorry for not reading the other issue close enough.

I am not sure I understand the memory usage though. Shouldn't this be somewhat streamable? The output size being this large makes total sense, but the resident memory should be manageable, right?

---

_Comment by @BurntSushi on 2023-05-05 15:14_

That's also discussed (very briefly) in #999 too. Output from each file is buffered in memory before being printed. There really isn't another choice if you want to prevent interleaving, which we do. If you disable parallelism then ripgrep will of course stream the output to stdout. For example:

```
$ time rg a --vimgrep > /tmp/rg2505-vimgrep.txt

real    6.080
user    0.959
sys     8.501
maxmem  18372 MB
faults  0

$ time rg -j1 a --vimgrep > /tmp/rg2505-vimgrep.txt

real    3.365
user    0.017
sys     3.340
maxmem  9 MB
faults  0
```

---

_Comment by @voidus on 2023-05-05 15:18_

Thank you for pointing this out again. I know how annoying this kind of work is sometimes so I really appreciate you taking the time to answer in this way.

Do you think it might be reasonable to make `-j1` default when `--vimgrep` is given? It seems to me that a bunch of people are running into this.

I personally thought it was just my 16Gigs beeing too little memory, so I upgraded and was a little surprised to see the same problem.

---

_Comment by @BurntSushi on 2023-05-05 15:24_

Hmmm. I don't know, to be honest. It's an interesting idea. I'll re-open the issue. I'll note the following though:

1. Text editor plugins and what not should be able to make the choice of whether to set `-j1` or not without ripgrep doing it.
2. It's not at all clear to me that making `-j1` the default is the right trade off. If 99.999% of all `rg --vimgrep` runs use "reasonable" memory, then setting `-j1` by default is likely to result in a _massive_ performance regression in the overwhelmingly common case.
3. `--vimgrep` is itself a strange thing, and is IMO legacy. It would probably be better if text editor plugins used ripgrep's `--json` flag to get structured matches. This is what VS Code does, for example.

---

_Reopened by @BurntSushi on 2023-05-05 15:25_

---

_Label `duplicate` removed by @BurntSushi on 2023-05-05 15:25_

---

_Label `question` added by @BurntSushi on 2023-05-05 15:25_

---

_Renamed from "--vimgrep memory usage again" to "should --vimgrep run in single threaded mode by default?" by @BurntSushi on 2023-05-05 15:25_

---

_Comment by @voidus on 2023-05-05 15:27_

Yeah I agree, it's a weird decision to make. For very short inputs, it's probably sensible, for longer ones it probably isn't. But making it conditional on the input length is weird.

I'll push to add `-j1` by default in the telescope threads, I think this is where most people are bitten by this since it searches on every input. So if you type `abc`, it'll kick off a `rg --vimgrep a` in the background (at least sometimes, I think)

Again, thank you for your answer even though it's an old topic <3

---

_Comment by @BurntSushi on 2023-05-05 15:40_

> So if you type `abc`, it'll kick off a `rg --vimgrep a` in the background (at least sometimes, I think)

Ah! I see. Yeah I don't actually use any ripgrep text editor plugins (except I guess for `fzf` but it only uses ripgrep to list out the files, not for searching), so I don't have a good sense of how they actually work. But yeah, indeed, `rg --vimgrep a` is a dangerous thing to run!

This is indeed a tricky situation. I do feel like `--json` is probably the right answer here. `--vimgrep` feels fatally flawed unfortunately. There are other various things that could be done, but they all feel yucky:

* As you mention, set `-j1` based on needle length. I'd venture `needle.len() <= 2` => `-j1` might be good enough. Once you get beyond two bytes (assuming it's a literal), match frequency tends to drop off quite a bit in non-pathological cases. Of course, you could run a regex like `.*`, but then you get what you deserve I guess? Hah.
* We could add an option to ripgrep that caps the size of the output buffer used by each thread. And if ripgrep would exceed that size it.... emits an error? (I don't like this for a lot of reasons.)
* Don't use `--vimgrep` and instead just parse the normal output. Maybe with `--column`? I don't know how these editor plugins work and whether they really really need `--vimgrep`. This might be easier than going full hog with `--json`.

---

_Comment by @voidus on 2023-05-05 15:45_

Good points, I guess maybe it's time to shift the whole ecosystem away from the --vimgrep format...

Without knowing the actual code or any of the pitfalls, wouldn't it be possible to have limited buffers with blocking until they are consumed? (I'm completely ignoring a possibly huge refactoring here of course)
I think the keyword is backpressure, but I only know it in the abstract, not if it would be a fit here or the intricacies of actually implementing it.

---

_Comment by @BurntSushi on 2023-05-05 15:55_

In the abstract it is theoretically possible I believe. Playing it forward: All of the matches from a particular file need to be emitted in one contiguous chunk. Backpressure could work if _one particular_ file resulted in a large output buffer and backpressure was based on _total_ memory consumption. In that case, you'd wait for some of the other output buffers to get printed and then allocate their space to the one that is stuck because it ran out of room. At that point, you'd need to refactor the code to dynamically switch to streaming _just_ the output of that one file to stdout, and once complete, switch back to the normal parallel strategy of buffering output. But what if there are multiple output buffers that are at or near capacity? You'd have to stop the entire search and focus on one buffer at a time until you've drained the buffers at capacity, and then resume the standard parallel search. In the worst case (and I kind of expect `rg a --vimgrep` to provoke exactly that), it will devolve to single threaded search, but with a lot of overhead.

The overall strategy is infeasible to implement from my perspective. More generally, adding backpressure to a system that wasn't designed for it at the start is quite difficult. And it probably doesn't really solve the problem here any better than using `-j1` does with one exception: it would dynamically switch to `-j1` only when needed. But popping up a level, all of this is a result of incidental complexity caused by how editor plugins use ripgrep in the first place. So it's definitely not worth adding backpressure IMO.

---

_Comment by @bluss on 2023-08-02 07:35_

Does --max-columns 4096 help on the test case for this issue? From my reading of my vim docs, grepprg functionality does not look past the first 4096 bytes of a line anyway. It seems like an option that can cut down the amount of work needing to be done here.

---

_Comment by @BurntSushi on 2023-11-24 19:06_

@bluss Interesting idea. I tried that out and it doesn't seem to help unfortunately.

I thought about this and I think I'm going to leave the behavior of `--vimgrep` alone. Disabling parallelism for that mode seems like a pretty huge bummer. But I have added some extra docs warning about these failure modes.

---

_Label `rollup` added by @BurntSushi on 2023-11-24 19:06_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---

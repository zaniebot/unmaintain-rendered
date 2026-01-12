```yaml
number: 955
title: "ripgrep doesn't use a buffered writer in single threaded mode (!)"
type: issue
state: closed
author: hgoscenski
labels:
  - bug
assignees: []
created_at: 2018-06-18T15:57:21Z
updated_at: 2018-06-24T00:49:08Z
url: https://github.com/BurntSushi/ripgrep/issues/955
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep doesn't use a buffered writer in single threaded mode (!)

---

_@hgoscenski_

#### What version of ripgrep are you using?

`λ rg --version                                                                                                        
ripgrep 0.8.1
-SIMD -AVX`

#### How did you install ripgrep?

Via brew.

#### What operating system are you using ripgrep on?

MacOS 10.13.5 (17F77)

#### Describe your question, feature request, or bug.

Performance of ripgrep seems to be substantially slower than GNU grep on a log file I am parsing. Logs are of the format (sanitized):
```
server.name.xz:Jun 06 00:02:35.172 server.name    [E] 2018/06/05 23:02:35.141 E 1c0e236fbb0cca535b176a83339eb 5254 13032 **************************************** 164ms Response:  Java::JavaNet::ConnectException Connection refused  Connection refused
```
File Size:
```
hgoscenski@hgoscenski-ltm:~/Downloads
λ ls -halt noc_9512v2.txt                                                                                             
-rw-r--r--@ 1 hgoscenski  staff    56M Jun 11 10:57 noc_9512v2.txt
```

#### Terminal output with rg, grep, ggrep
https://gist.github.com/hgoscenski/059ab6394b3c0520e37d34c448b69d98


---

_Comment by @BurntSushi on 2018-06-18 16:04_

Neat! Thanks for filing this sort of issue. I'd love to do an analysis, but I can't really do anything without the corpus. Can you provide one? If you can't share the one you described, can you reproduce the performance issue on something we can both see?

If not, I'll unfortunately just have to close this.

---

_Label `question` added by @BurntSushi on 2018-06-18 16:04_

---

_Comment by @hgoscenski on 2018-06-18 16:43_

Can you agree to destroy the corpus after testing? It should be relatively sanitized but at the same time it is from my company logs and I would rather not have it floating about. 

-Harrison

> On Jun 18, 2018, at 11:04, Andrew Gallant <notifications@github.com> wrote:
> 
> Neat! Thanks for filing this sort of issue. I'd love to do an analysis, but I can't really do anything without the corpus. Can you provide one? If you can't share the one you described, can you reproduce the performance issue on something we can both see?
> 
> If not, I'll unfortunately just have to close this.
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub, or mute the thread.


---

_Comment by @BurntSushi on 2018-06-18 17:49_

@hgoscenski Yeah, that's fine. It's probably too big to email, but if you upload it somewhere (that you can then delete after I download) and email me the link, then I can do that. I will make sure to delete whatever you give me before closing this issue. My email is on this page: https://blog.burntsushi.net/about/

---

_Comment by @BurntSushi on 2018-06-18 21:28_

Nice find! I was able to reproduce this in a more emphatic way on the open subtitle data set:

```
$ time rg -v 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en | wc -l
32721743
real    0m19.855s
user    0m9.368s
sys     0m30.039s
$ time egrep -v 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en | wc -l
32721743
real    0m2.679s
user    0m2.817s
sys     0m1.060s
```

If we suppress output and just count the matches, then all is well in the world:

```
$ time rg -cv 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en
32721743
real    0m0.562s
user    0m0.517s
sys     0m0.044s
$ time egrep -cv 'Sherlock Holmes' OpenSubtitles2016.raw.sample.en
32721743
real    0m0.872s
user    0m0.751s
sys     0m0.120s
```

So this is seemingly a performance bug in the printer? Well, if that were true, then `rg .` should be just as slow:

```
$ time egrep . OpenSubtitles2016.raw.sample.en | wc -l
32722360
real    0m3.158s
user    0m3.332s
sys     0m1.098s
$ time rg . OpenSubtitles2016.raw.sample.en | wc -l
32722360
real    0m25.141s
user    0m12.447s
sys     0m35.610s
```

... yikes. That's no good. This is specifically a case that I tested when initially building ripgrep. My guess is that there is a regression lurking somewhere. So, I did some time travel. It turns out that ripgrep 0.2.9 doesn't have this regression (so my recollection is still in tact, it seems), but ripgrep 0.3.0 does:

```
$ rg --version
ripgrep 0.2.9
$ time rg . OpenSubtitles2016.raw.sample.en | wc -l
32722360
real    0m3.054s
user    0m3.393s
sys     0m0.769s

$ rg --version
ripgrep 0.3.0
$ time rg . OpenSubtitles2016.raw.sample.en | wc -l
32722360
real    0m25.367s
user    0m11.428s
sys     0m37.080s
```

OK, so bisecting between these two tags, it appears that commit e8a30cb8934106e63f9d0938c547e535acb7b888 introduced the regression. This makes sense to an extent, since this is the commit that re-worked colored output and tty handling. My suspicion at this point is that I somehow disabled buffered writing in the single threaded case.

Now that I have an idea of what the bug is, I'm looking more closely on master. Indeed, this is how we get the stdout handle:

```
    pub fn stdout(&self) -> termcolor::StandardStream {
        termcolor::StandardStream::stdout(self.color_choice)
    }
```

All this does is use `io::Stdout` internally on Unix, which isn't buffered at all.

Sigh. This is a bad bug. I am really surprised that this hasn't been caught. I suppose this somewhat validates the assumption that ripgrep's output is often much much smaller than what it searches.

Technically, this bug is also present in the multithreaded searcher, but is less pronounced because the multithreaded searcher buffers the entire output of each file in memory before printing it.

This will be interesting to fix because of colors... Brainstorming:

* Handling the ANSI color escapes with a normal buffer writer is simple. Windows is the problem. So we could just fix this for ANSI escapes.
* Add a new buffered writer API to wincolor that probably uses the `BufferWriter` internally. This is probably the best route to go.

---

_Comment by @BurntSushi on 2018-06-18 21:28_

@hgoscenski I've deleted your data from my system.

---

_Label `bug` added by @BurntSushi on 2018-06-18 21:28_

---

_Label `question` removed by @BurntSushi on 2018-06-18 21:28_

---

_Renamed from "GNU grep 2x as fast as ripgrep on corpus. " to "ripgrep doesn't use a buffered writer in single threaded mode (!)" by @BurntSushi on 2018-06-18 21:30_

---

_Comment by @hgoscenski on 2018-06-19 01:36_

@BurntSushi thank you! Let me know if you need anything else from me.

---

_Closed by @BurntSushi on 2018-06-24 00:49_

---

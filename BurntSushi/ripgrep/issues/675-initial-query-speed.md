```yaml
number: 675
title: Initial query speed
type: issue
state: closed
author: ianchanning
labels:
  - question
assignees: []
created_at: 2017-11-11T16:15:19Z
updated_at: 2017-11-14T16:24:32Z
url: https://github.com/BurntSushi/ripgrep/issues/675
synced_at: 2026-01-12T16:13:22Z
```

# Initial query speed

---

_@ianchanning_

Thank you for this awesome tool! I'm using it daily in Vim.

I'm using ripgrep 0.7.1 on Windows. It is taking 10s to search a directory with ~26,000 files (of which ~23,000 files are in the `node_modules` directory - which I'm trying to ignore)

```
C:\Users\Ian>rg --version
ripgrep 0.7.1
-AVX -SIMD
```

I installed it from the [chocolatey package][1]. I am running on 64-bit Windows 10, with 8 GB RAM and an Intel i5-3320M 2.6 GHz CPU.

Running the following command initially takes 9 - 10 seconds to fully complete:

```
rg --vimgrep -tphp --type-add "ctp:*.ctp" -tctp --glob !.svn --glob !node_modules
```

This is still faster than grep, so I have zero complaints, but I'm trying to figure out if I'm doing something wrong or if this is the expected performance.

This purely seems to be some initial index building, once that is done, all queries are sub-second.

Please let me know if there are some better debug info that I can give you.

  [1]: https://chocolatey.org/packages/ripgrep

---

_Comment by @BurntSushi on 2017-11-13 11:42_

> This purely seems to be some initial index building, once that is done, all queries are sub-second.

ripgrep builds no indices, but your intuition might not be that far off.

Firstly, if the first search is slow and all subsequent searches are fast, then what specific problem are you reporting?

Second, this particular behavior is indicative of ripgrep needing to read your corpus from disk in order to search it. Once it's done the first time, the operating system will usually keep those files in a cache in RAM, which makes subsequent searches much faster. Most codebases these days fit into memory and most folks probably search their codebase multiple times, so the "search while everything is in cache" is the common case.

When files need to be read from disk, then most tools will likely perform at similar speeds, since they tend to be blocked on I/O.

Thirdly, why are you explicitly ignoring `node_modules`? Isn't that directory in your `.gitignore`? Similarly, `.svn` is hidden, so it is ignored by default.

Anyway, I'm not entirely sure what you're asking here. If you just want to know how everything works, then read the blog post: http://blog.burntsushi.net/ripgrep/

---

_Label `question` added by @BurntSushi on 2017-11-13 11:42_

---

_Comment by @ianchanning on 2017-11-13 22:00_

Thanks for your reply! Mostly this is just rookie fumbling, trying to see if I'm doing something obviously wrong.

I now understand what you mean that it's loading into RAM vs file access. The search is phenomenal once it is loaded into RAM and works amazingly for repeated queries.

You're right I don't need to include `.svn`, my mistake that was just from experimenting. I excluded `node_modules` because where as you respect `.gitignore` I don't think you take into account `svn:ignore` and I'm using subversion. I could create a `.gitignore`, but there's only the one directory I need to ignore and it's just the command I store in my `.vimrc`.

I was just a bit confused by my experience of taking 9-10s for searching 3,000 (100 MB worth) files vs the various performance metrics you use which you have e.g. 0.3s (http://blog.burntsushi.net/ripgrep/#linux-literal-default) for multiple GB worth of project directory. I've got a fairly old ThinkPad T430 with an SSD but I imagine that the specs of the your machine vs mine plus Windows vs Linux I/O speeds would explain the performance differences.

This initial search is only for the first search that I'll do each day or after starting from sleep / hibernate. But it was a daily occurance, so my mind wandered whilst waiting for the initial query :) I'd have thought that it should stick around in RAM longer though, so on starting from sleep / hibernate I thought that it loads what was in RAM back into there. But it seems to me that the RAM being used for ripgrep is getting cleared each time I close my laptop.

I think one of the issues is purely perceived performance. Which I've figured out now is due to Vim's synchronous nature by default. If you do a search using ripgrep in Vim nothing happens until the search is complete. However using the AsyncRun plugin for Vim 8, you start getting results back much quicker.


---

_Comment by @BurntSushi on 2017-11-14 12:20_

>  don't think you take into account svn:ignore and I'm using subversion.

Ah I see, yes, that's correct.

> I was just a bit confused by my experience of taking 9-10s for searching 3,000 (100 MB worth) files vs the various performance metrics you use which you have e.g. 0.3s (http://blog.burntsushi.net/ripgrep/#linux-literal-default) for multiple GB worth of project directory. I've got a fairly old ThinkPad T430 with an SSD but I imagine that the specs of the your machine vs mine plus Windows vs Linux I/O speeds would explain the performance differences.

You read the methodology section, right? It very carefully explains that the benchmarks are setup such that the cache is warm before extracting a sample time.

With that said, taking 10s to search 100MB on an SSD does indeed sound egregiously bad. I have the exact same laptop as you (with an SSD, using Linux), and that kind of timing isn't what I'd expect. Unfortunately, I don't really have any way to debug it without being able to reproduce it, so I think you're pretty much on your own here. You'd need to investigate yourself and figure out the source of the problem. If I might brainstorm some ideas for you:

1. Consider more rigorously testing other tools and compare timings. If search is truly blocked on I/O, then any non-toy search program should take roughly the same amount of time.
2. Consider testing tools that just crawl the directory and print file names without looking at the contents of a file (e.g., `find`). If that is also very slow, then that gives some clue that perhaps something is funky with directory traversal.
3. Consider controlling your environment more rigorously. Do you have other programs running that are eating up CPU or memory while executing a search?
4. Are you *sure* that you're only searching 100MB? Maybe ripgrep is actually looking in `node_modules` after all? You can check by running ripgrep with the `--files` flag.
5. On Linux, I know how to flush the I/O cache such that the next time I run ripgrep, the OS will be forced to read from disk. You might consider figuring out how to do this on Windows so that it's easier to test your problem. Having to suspend or reboot your computer for every single test isn't practical.

None of these things will necessarily give you the answer you seek, but they will help to form a more complete picture of what's going on. If you report your results back here, then it might help further.

> I'd have thought that it should stick around in RAM longer though, so on starting from sleep / hibernate I thought that it loads what was in RAM back into there. But it seems to me that the RAM being used for ripgrep is getting cleared each time I close my laptop.

Yeah I don't know anything about that when it comes to Windows, sorry.

---

_Comment by @ianchanning on 2017-11-14 16:17_

Brilliant, thankyou. That's actually I guess what I was after, a sensible set of tests that I can run as I don't know enough about memory issues. I did a quick check and the queries are still coming from RAM after briefly putting the laptop to sleep and waking it again.

I've got a dual boot with Fedora, so I'll do some performance checks against my code base on there.

But for now I think this can be closed, you've explained a lot! I'll add any results I get back.

---

_Closed by @ianchanning on 2017-11-14 16:17_

---

_Comment by @BurntSushi on 2017-11-14 16:24_

Sounds good! Note that on Linux, you can flush the page cache with `sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'`.

---

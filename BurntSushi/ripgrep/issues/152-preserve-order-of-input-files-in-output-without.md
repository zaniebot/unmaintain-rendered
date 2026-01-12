```yaml
number: 152
title: preserve order of input files in output without sacrificing parallelism
type: issue
state: closed
author: jikamens
labels:
  - enhancement
assignees: []
created_at: 2016-10-05T23:14:59Z
updated_at: 2021-06-23T16:36:40Z
url: https://github.com/BurntSushi/ripgrep/issues/152
synced_at: 2026-01-12T16:13:21Z
```

# preserve order of input files in output without sacrificing parallelism

---

_@jikamens_

There should be a way to tell rg to preserve the order of input files specified on the command line in the output.

E.g., if I'm searching a bunch of files which are in chronological order, I want the output to be in chronological order.


---

_Comment by @BurntSushi on 2016-10-05 23:25_

You can do this today, but you have to give up parallelism. If you run `rg -j1 PATTERN FILE FILE ...` then the output should be deterministic and in the order specified. In general, when recursively searching a directory, the order is whatever the file system serves up, but with `-j1` it should at least be deterministic.


---

_Comment by @jikamens on 2016-10-06 00:06_

Yeah, so I don't _want_ to give up parallelism, obviously.

[GNU parallel](https://www.gnu.org/software/parallel/) let's me be parallel and preserve the order of output with `--keep`. I'm proposing that rg should provide similar functionality. Otherwise `rg -j#` isn't a drop-in replacement for grepping a bunch of files with GNU parallel, which I would really like it to be. ;-)


---

_Comment by @BurntSushi on 2016-10-06 00:15_

I get what you're saying. Here are my thoughts on the matter:
1. If deterministic ordering + parallelism were possible to do without incurring additional costs, `rg` would do that by default. (Because I'd really like deterministic ordering too.) Perhaps I am wrong and this _is_ possible (or perhaps, possible with immeasurable costs), and if so, we should do that.
2. If not (1), then hiding this behind another flag seems like bad UX, but maybe that's the best we can do.
3. Requiring single threaded searches for when you absolutely need deterministic output doesn't seem unreasonable to me. For example, `rg` can sometimes beat or be competitive with `ag` using a single thread even when `ag` runs with multiple threads.

All of these things combined together means I'm not particularly motivated to work on this, but I grant it's something I'd be willing to explore given unbounded resources.


---

_Label `enhancement` added by @BurntSushi on 2016-10-06 00:15_

---

_Comment by @jikamens on 2016-10-06 00:27_

You can implement ordering + parallelism with no additional cost; all rg has to do is read output from the threads in the order that they are launched. This means, of course, that some of the threads will block waiting to be read because their output buffers will fill up, but the performance of such an implementation would be no _worse_ than `-j1`, and in most cases it would be significantly better.

You can improve the performance simply by making the output buffers larger so that they block less.

"You just have to put up with your search running eight times slower on your eight-core machine," is not really a reasonable answer, especially since I _don't_ have to put up with it, since as I pointed out, there are other tools that _will_ allow me to search and preserve ordering in the output.

I frankly get the impression that you are rationalizing this idea being not worthwhile because the internal implementation of rg is not conducive to it. That's putting the cart before the horse. If it's too hard to do this right now in rg, fine, that's a reasonable answer. But to then try to hand-wave away the value of the idea because it's hard to do is a step too far.


---

_Comment by @BurntSushi on 2016-10-06 00:49_

> You can implement ordering + parallelism with no additional cost; all rg has to do is read output from the threads in the order that they are launched. This means, of course, that some of the threads will block waiting to be read because their output buffers will fill up

Unless I'm missing something, it sounds like the additional cost there is both memory and time.

> but the performance of such an implementation would be no worse than -j1, and in most cases it would be significantly better.

But it sounds like it will be slower than `-j8` on an eight core system, which doesn't quite satisfy "no additional cost." With that said, it's plausible that there is no measurable cost, but it's hard to know that without trying an experiment.

Comparing your suggestion to `-j1` is interesting, but that's not the standard we need to live up to make this the default behavior. Simply being better than `-j1` is good enough to put it behind a flag, but it's not good enough to make it default. I know you requested putting it behind a flag, but I'd _like_ to see it be default behavior, and this is the frame of mind I have when inquiring about costs.

> "You just have to put up with your search running eight times slower on your eight-core machine," is not really a reasonable answer, especially since I don't have to put up with it, since as I pointed out, there are other tools that will allow me to search and preserve ordering in the output.

Of course it's a reasonable answer. This is about time preferences and priorities. Two reasonable people can disagree on how important this particular issue is. I happen to think it's not that important. You clearly do. That's OK.

If GNU parallel lets you achieve this already, then it sounds like you can use `ripgrep` with GNU parallel to get everything you want except for the convenience of using one tool instead of two.

> I frankly get the impression that you are rationalizing this idea being not worthwhile because the internal implementation of rg is not conducive to it.

I'm sorry I gave you that impression because that wasn't my intent. I think we should try to avoid reading into others' motivations and give the benefit of the doubt that I'm saying what I mean. Without an assumption of good faith here, it's hard to have a productive conversation. (To be frank, this comment by you is putting me on the defensive and making it hard for me to respond.)

To respond though, I don't think `rg`'s current implementation really has anything to do with this. The multithreaded component of `rg` is pretty small (look at `src/main.rs`), and it wouldn't take much effort to rewrite it. In fact, I plan on rewriting at least a portion of it soon, because I'd like to make directory iteration itself parallelized. I suspect this may make deterministic output even harder. The best I can do is say that I'll give this issue more thought when I parallelize the directory iterator.


---

_Comment by @jikamens on 2016-10-06 00:57_

Point taken. Thanks for the thoughtful response.


---

_Comment by @jikamens on 2016-10-06 00:57_

> If GNU parallel lets you achieve this already, then it sounds like you can use `ripgrep` with GNU parallel to get everything you want except for the convenience of using one tool instead of two.

Unfortunately, I frequently get "Out of memory (os error 12)" from ripgrep when I try to use "rg -j1" with GNU parallel.


---

_Comment by @BurntSushi on 2016-10-06 01:00_

@jikamens Well... that's bad. :P If you'd be willing to file a bug, I'd be happy to look into it. I'm not a user of GNU parallel, so I think I'd need at least the full command you're running. If you can reproduce the issue on data we can both access, that's even better, but I understand if that's not possible.


---

_Comment by @jikamens on 2016-10-06 01:06_

I've just told you all that I'm able to tell you, so you can file the bug yourself if you want. ;-)

Seriously, the reason I haven't filed a bug is because the data I'm searching is very large and proprietary and so are the search strings I'm using, so all I'd be able to say in the bug is, "Yeah, I'm getting `Out of memory (os error 12)` but I can't tell you want I'm doing to provoke it." If I have time and am able to construct a test case that I can share, I'll open an issue.


---

_Renamed from "Should be a way to preserve order of input files in output" to "preserve order of input files in output without sacrificing parallelism" by @BurntSushi on 2016-10-11 00:09_

---

_Comment by @ryanberckmans on 2017-02-08 21:34_

@BurntSushi on ripgrep 0.4.0,

```
       --sort-files
              Sort results by file path.  Note that this currently disables 
              all parallelism and runs search in a single thread.
```

It would be great if `--sort-files` **with** parallelism was the default. Sorting results by file path is a big benefit for me. Let me know if there's a better place for this feedback :). 

Thanks for all your hard work! ripgrep is a joy to use. We have type coupling between microservices so I am often searching for `FooRequestType` and rely on file path order to parse results by service.

---

_Comment by @BurntSushi on 2017-02-08 21:38_

@ryanberckmans There's no debate that `--sort-files` with parallelism would be better than `--sort-files` without parallelism **from an end user's perspective**. I mean, it's not like I just arbitrarily chose to disable parallelism for the hell of it.

Glad you're enjoying ripgrep. :-)

---

_Comment by @BatmanAoD on 2017-03-30 18:25_

Since the documentation states that search time is guaranteed to be linear, perhaps we'd get *more* consistency (without guaranteeing full determinism of output) by ordering the files to search from largest to smallest by default? This of course would require completing the directory traversal before even starting a search, but I'm not sure how big a burden that is, either in terms of programming effort to make the modification or runtime memory usage to store and traverse the file list. (I'm guessing that `--sort-files` does *not* have this limitation, since you don't need to inspect child directories before starting to search through files in a parent directory.)

Obviously, to *guarantee* consistency, there's no way to escape the fact that sometimes the output will "hang" while waiting for the next thread to complete its search.

---

_Comment by @BurntSushi on 2017-03-30 18:46_

Storing the entire tree in memory isn't a path I'm excited about pursuing. It's not only a huge code change, but it will introduce additional latency and a memory requirement that is proportional to the size of the tree.

---

_Comment by @BatmanAoD on 2017-03-30 19:03_

That makes sense.

Does ripgrep currently search all files in a directory before going into subdirectories? If so, simply sorting the files in *each* directory by size (largest first) after getting the listing from the file system seems like it would probably be an improvement. (I'm guessing that getting the total size of each child directory is probably not worthwhile if exploring the entire file tree isn't an option.)

---

_Comment by @BurntSushi on 2017-03-30 19:07_

@BatmanAoD The directory traversal *itself* is parallelized. It's a key reason why ripgrep is so fast even when it needs to process gitignore files. Regardless, standard directory traversal is depth first, not breadth first, precisely for memory constraints. (Large but flat directories are more common than exceedingly deep directories.)

---

_Comment by @BatmanAoD on 2017-03-30 21:24_

Right, but there's still an ordering of tasks available for execution (in parallel), based on which files haven't been searched yet and which folders haven't been explored yet. And even in a depth-first search, the full list of entries in a single directory is still retrieved from the file system before the children are actually searched (or added to the work queue). So I'm suggesting that in between the call to `fs::read_dir` (or, probably, after applying ignore rules) and the actual insertion of directory entries into the work queue, the contents of the directory are sorted against each other in descending size (possibly treating all directory entries as "larger" than file entries). (I'm not suggesting any kind of sorting between files or directories with different parent directories.)

......but now that I've written that out, I think the obvious question I should have asked earlier is, what kind of work queue does ripgrep actually use? If it's simply FIFO, perhaps switching to a priority queue based on size would be the best way to ensure that larger files are searched first if possible?

---

_Comment by @BurntSushi on 2017-03-31 01:23_

> And even in a depth-first search, the full list of entries in a single directory is still retrieved from the file system before the children are actually searched (or added to the work queue).

Nope. :-) In depth first search, the only things you need to store in memory are:

1. The current entry (which is roughly equivalent to the `dirent` structure specified in `man 3 readdir` on Linux).
2. A stack of the directories one has descended into (which is usually on the heap in a non-toy implementation). Each entry in the stack is an open file descriptor that reads the contents of a directory as a stream. (e.g., The return value of `man 3 opendir`.)

With that said, for a directory tree of depth `N`, this requires one to have `N` open file descriptors. On Linux, the default file descriptor limit is low enough that it's feasible to hit this, so most non-toy implementations [will cap the maximum number of open file descriptors](https://docs.rs/walkdir/1.0.7/walkdir/struct.WalkDir.html#method.max_open). Once the maximum is reached, the file descriptor at the top of the stack is exhausted (by reading the rest of its stream and storing it into memory) and then closed, which frees up a file descriptor.

At least, this is roughly how `walkdir` works, which is what ripgrep uses for its single threaded search. In prior versions, ripgrep also used `walkdir` for the parallel search, and it worked quite simply. Here's some pseudo code:

```
workq := Queue::new();
main thread:
    let mut workers = vec![];
    for i in number_of_cpus() {
        workers.push(Worker::new(workq.clone()));
    }
    for entry in WalkDir::new("./") {
        if should_search(&entry) {
            workq <- entry
        }
    }
    workq.close()
    for worker in workers {
        worker.join().unwrap();
    }

worker thread:
    for entry in workq {
        let results = do_search(entry);
        let stdout = io::stdout().lock();
        print(stdout, results)
    }
```

And that was pretty much it. The `walkdir` crate itself can be made to [sort the contents of a directory](https://docs.rs/walkdir/1.0.7/walkdir/struct.WalkDir.html#method.sort_by), which would give us a deterministic order *in the main thread*. In this scheme, I believe it's possible to impose a total ordering in the main thread by assigning a sequence number to each `entry`. So if `walkdir` itself yields entries in a deterministic order, then we can tag each entry so that each entry knows the order in which it should be printed.

The worker thread could then be made to "block" until it was the entry's turn to be written, which could be done by checking the sequence id of the most recently printed entry. (There's probably a bit more to this, and perhaps latency could be reduced by introducing a ring queue and another worker that is only responsible for printing. This last worker would need to do a bit of windowing not unlike how TCP works.)

I'd also like to take this time to note that "sorting by file size" is itself a non-starter because it would slow ripgrep down a bit. Namely, ripgrep would need to issue a stat call for each file.

With that said, ripgrep no longer uses this simplistic approach. The core problem with it is that it forces the processing of all filter logic to be single threaded. Given the [size of some gitignore files](https://github.com/mysql/mysql-server/blob/5.7/.gitignore), this can end up being a substantial portion of ripgrep's runtime. Thus, a while ago, I set out to parallelize directory traversal itself. It still uses a work queue, but the fundamental difference is that there are no longer separate producers or consumers. Instead, every producer is, itself, a consumer. The reason for this is that if you're parallelizing directory traversal, then you end up reading the contents of multiple directories simultaneously, so there's no single point of truth about when it terminates. Indeed, [termination is complicated](https://github.com/BurntSushi/ripgrep/blob/71585f6d476265338e9ee4e924b4f43b02f2d233/ignore/src/walk.rs#L1159-L1220).

The big picture problem is that this form of parallelism makes determinism much harder than the simpler work queue formulation that ripgrep used to have. In particular, each item in a directory is [sent along a queue](https://github.com/BurntSushi/ripgrep/blob/71585f6d476265338e9ee4e924b4f43b02f2d233/ignore/src/walk.rs#L1083), which means there is no point at which you could sort the files you're searching and realistically expect that to result in more deterministic output. Namely, even if you collected all of the entries of a directory and sorted them before sending them to the queue, you would in fact be racing with the N other threads that are doing the same.

To solve the deterministic output problem while simultaneously using parallelism, I think you need to do one of these things:

1. In addition to the single threaded and multithreaded search ripgrep has today, add a third mechanism that looks like the more traditional work queue style and implement deterministic output using that. You'll give up parallel processing of ripgrep's filters, but you'll still get parallel search. (I think this is inevitably what we'll do.)
2. Learn how ripgrep does parallel search right now and figure out how to make its output deterministic without imposing significant costs. (I don't think this is possible. I think you could assign sequence ids to each entry in a way that imposes a total ordering across all entries, but the traversal fans out so quickly that I fear this would essentially be equivalent to "buffering all output." Incidentally, this will work in a lot of cases, but it will also mean that ripgrep's memory usage will spike. I am proud of ripgrep's current memory usage.)
3. Come up with a completely different approach to parallelizing search. I've talked with a few folks about this, and they suggested a work stealing approach might also work, but I don't know if that makes sorting the output any easier. (This is also much more complex. They cited the "goroutine scheduler in the Go runtime" as the place to look.)
4. Change the requirements. e.g., Convince me the buffering all of ripgrep's output would be OK.

---

_Comment by @BurntSushi on 2017-03-31 01:31_

> Come up with a completely different approach to parallelizing search.

If you look at my pseudo code above, you might wonder why one couldn't just move the `should_search` filter into the worker. Indeed, this could solve a lot of the problems. The problem there is that you wind up with a lot of synchronization overhead associated with maintaining the filters. (The filters are expensive to construct, so you need to cache them.) In particular, checking a filter may actually involve ascending the directory stack to check filters associated with parent directories. (For example, to correctly match `.gitignore` files.)

So if you go down this route, you basically need to move construction of the filters themselves into the workers as well. But if you do this, you either need to be willing to pay for constructing each filter N times (for each of the N threads), or you need to do some type of synchronization so that each filter is built once. If you pick the former, then you're right back to where you started: you're not actually parallelizing a good chunk of the filter process, although you are parallelizing the filter *match* process, which is a small win. If you pick the latter, then at some point, you're going to ask `<=N` workers to work on `M` files from the same directory, which means all `M` workers will need to sit and wait until the filter for that directory is built. Which, of course, also destroys parallelism.

If you really tumble down the rabbit hole, then maybe there is a smarter way to prioritize which entries get searched based on which filters are available, but then you've probably given up determinism again.

If you get this far, then you're back to door number 3 in my previous comment.

---

_Comment by @BatmanAoD on 2017-03-31 05:30_

Okay, I think I understand much better now what you meant by "the directory traversal itself is parallelized" and why that presents an essentially intractable problem for obtaining determinism via the sort of minor tweaks I was trying to propose. And at the risk of making my complete ignorance of the issues at play even more obvious (! :laughing:), I didn't even realize that directories could be read as streams in Linux, let alone in any sort of cross-platform application (at least without circumventing the OS filesystem API and reading/interpreting the raw bits).

...and, looking back, I'm not even sure why I suggested that largest-file-first would be valuable, even heuristically; I think my idea was that this would prioritize actual regex-matching over output printing, but I'm pretty sure that's exactly the reverse of what you'd want.

Based on the very high-level explanation of Go-routine scheduling and stealing [here](http://morsmachine.dk/go-scheduler), I do not immediately see any reason why that form of scheduling would make output-sorting any easier.

---

_Comment by @BatmanAoD on 2017-03-31 05:30_

Anyway, thanks for the detailed explanation! That's quite helpful in understanding what's going on.

---

_Comment by @bmalehorn on 2017-04-16 08:21_

Hi all.

I also want this feature for ripgrep, as it's one of my most missed features from `ag`. I've read over this whole thread and I think I have a good idea of the problem, and I have an idea on how we can fix it.

## Priority Queue Solution

Suppose you're searching this directory:

     1.txt
     2/2.txt
     3.txt

Let's say we sort the output of readdir.
Currently, that would make it search like this:

    queue                         searched
    -----                         --------
    ["1.txt", "2/", "3.txt"]
    ["2/", "3.txt"]               1.txt
    ["3.txt", "2/2.txt"]          1.txt
    ["2/2.txt"]                   1.txt, 3.txt
    []                            1.txt, 3.txt, 2/2.txt

I propose we use a priority queue, ordered by path name:

    priority queue                searched
    --------------                --------
    ["1.txt", "2/", "3.txt"]
    ["2/", "3.txt"]               1.txt
    ["2/2.txt", "3.txt"]          1.txt
    ["3.txt"]                     1.txt, 2/2.txt
    []                            1.txt, 2/2.txt, 3.txt

Notes about this approach:

- Relatively small change. Swap queue for priority queue.
- Won't guarantee ordering with >1 thread.
- This approach is only a heuristic that gets us much closer to sorted output.
- Hopefully this heurstic will be good enough.
- Might degrade performance - trading lockless queue for locked priority queue.
  
Questions for @BurntSushi:

Does this make sense? Do you agree that this will mostly sort the output? Or do you see some critical flaw I didn't consider?

Is this something you'd be willing to merge? Assuming everything pans out - files are usually sorted and there's no major performance change.


---

_Comment by @BurntSushi on 2017-04-16 11:31_

@bmalehorn did you happen to read my most recent comments on this? Namely, i don't see how this helps the matter. Remember, in the current implementation, the consumer of the queue is also its producer.

Also, I'm pretty sure ag doesn't have this feature. Perhaps you could elaborate on that point?

---

_Comment by @BurntSushi on 2017-04-16 11:37_

The other issue I see is that your queue example seems to indicate a breadth first search. But most recursive directory iterators use depth first search (including ripgrep). Which kind of makes the priority queue idea moot.

---

_Comment by @bmalehorn on 2017-04-16 18:03_

> Also, I'm pretty sure ag doesn't have this feature. Perhaps you could elaborate on that point?

`ag` will search in whatever order readdir(2) emits. On OS X this is sorted order. On Linux, you can sort the output of readdir with a [small patch](https://github.com/ggreer/the_silver_searcher/pull/942/files).

> But most recursive directory iterators use depth first search (including ripgrep).

Actually, I don't think ripgrep uses depth first search.

`walk.rs` uses a `crossbeam::sync::MsQueue`, which is a queue. I tried adding some print statements, and it indeed seems to be doing breadth first search.

I tried swapping it out for a stack, and sorting readdir:

    $ rg -l PM_RESUME
    Documentation/dev-tools/sparse.rst
    Documentation/translations/zh_CN/sparse.txt
    arch/x86/kernel/apm_32.c
    drivers/ide/ide-pm.c
    drivers/input/mouse/cyapa.h
    drivers/mtd/maps/pcmciamtd.c
    drivers/net/wireless/intersil/hostap/hostap_cs.c
    drivers/usb/mtu3/mtu3_hw_regs.h
    include/uapi/linux/apm_bios.h
    include/linux/ide.h

The result is that output is often sorted. However, it can get out of order if, for example:

1. `Documentation` is popped by thread 1
2. `arch` is popped by thread 2
3. children of `Documentation` pushed by thread 1
4. children of `arch` are pushed by thread 2

Now you will interleave `Documentation` and `arch`. I got such an out-of-order case by running `cd ripgrep && rg -l ack`.

You can view the commit [here](https://github.com/bmalehorn/ripgrep/commit/7088299fee0c1c6959f28be1a56d424485c5b5a8).

Even if you don't want to use my half-assed sorting approach, I'd still argue that ripgrep should use a stack instead of a queue. The stack won't grow nearly as large as the queue.

---

_Comment by @BurntSushi on 2017-04-16 18:17_

> ag will search in whatever order readdir(2) emits. On OS X this is sorted order. On Linux, you can sort the output of readdir with a small patch.

Uh... Maybe in single threaded mode, but certainly not when search is parallelized. `ag` has non-deterministic output order, just like ripgrep.

> walk.rs uses a crossbeam::sync::MsQueue, which is a queue. I tried adding some print statements, and it indeed seems to be doing breadth first search.

I see. I was thinking about `walkdir`, sorry, which is depth first.

> You can view the commit here.

> Even if you don't want to use my half-assed sorting approach, I'd still argue that ripgrep should use a stack instead of a queue. The stack won't grow nearly as large as the queue.

The sorting approach is probably something that would need good benchmarking before getting merged. Particularly on very large directories. If ripgrep were used on a directory with many files, then it won't even start searching until the entire list is in memory.

If using a stack ends up with a more consistent order in practice, then I'd be fine with that!

---

_Comment by @bmalehorn on 2017-04-16 20:02_

> `ag` has non-deterministic output order, just like ripgrep.

To be clear, they're *both* non-deterministic. But there are a few "levels" of how non-deterministic their outputs are:

1. "completely ordered": No matter what the files will be in sorted order. Example: `git grep`
2. "start searching in order": Files will *commence* being searched in order. So "1.txt" will get passed to a worker thread before "2.txt". But if the match in "1.txt" is 1 GB into the file but the match in "2.txt" is 1 KB into the file, "2.txt" will probably print first. Example: `ag`
3. "unordered": No attempt to order outputs. Example: `ripgrep`

In practice, 2 is good enough. I've used this patched version of `ag` for months and have never observed files out of order.

"Priority queue" and "stack + sort readdir" are somewhere between 2 and 3.

----

I've made a PR for switching queue -> stack because it has some performance gains. It doesn't attempt to put the files in order.

----

> The sorting approach is probably something that would need good benchmarking before getting merged. 

Yeah I agree. I'm going to play around with sorting and priority queues and see what I find. Hopefully, we can either get sorting as the default, or make `--sort-files` compatible with (some) multithreading.

PS. Thanks for being such a responsive maintainer!

---

_Comment by @bmalehorn on 2017-04-25 22:35_

I've created two builds: `priority queue` (my creation), and `single producer` (earlier suggestion). In practice, both sort results. Benchmark:

    linux_literal (pattern: PM_RESUME)
    ----------------------------------
    rg (master)*           0.423 +/- 0.037 (lines: 16)*
    rg (priority queue)    0.431 +/- 0.039 (lines: 16)
    rg (single producer)   0.590 +/- 0.018 (lines: 16)
    rg (--sort-files)      0.954 +/- 0.034 (lines: 16)

In my opinion, the minor performance loss of `priority queue` is worth it to get sorted files. But you might disagree. Thoughts? Something else you want me to measure?

---

_Comment by @BurntSushi on 2018-08-22 12:56_

Realistically speaking, I don't think this feature request is going to get satisfied any time soon. If deterministic ordering is important to you, then for now you'll need to use `--sort-files` and live with single threaded search.

---

_Closed by @BurntSushi on 2018-08-22 12:56_

---

_Comment by @MightyCreak on 2018-11-06 16:58_

I would also be interested by this feature. As a programmer myself I would see a way to solve that. It would indeed have a cost, but it would be way better than `--sort` does right now (i.e. disabling parallelism entirely).

But you seem to have had a lot of discussion about that, could you help me with a TL;DR of this discussion about why it's impossible to do?

Thank you very much

---

_Comment by @BurntSushi on 2018-11-06 17:19_

> could you help me with a TL;DR of this discussion about why it's impossible to do?

I never said it was impossible, so I'm confused as to why you're asking this question. This feature is precisely about trade offs, in terms of runtime performance and also in terms of implementation complexity.

Honestly, I haven't thought about this specific problem in a long time, so I would need to re-read this thread myself to give you a convincing TL;DR. But given that it wouldn't be answering the question you asked, I think it would be better if you just went ahead and read the thread. If you have specific questions after that, then I'd be happy to try and answer those.

---

_Comment by @MightyCreak on 2018-11-06 21:35_

Right, wrong wording, I considered that if the issue's been closed it meant that after the discussion you arrived to the conclusion that the trade offs were not good enough to spend more time on it.

No worries if you don't remember the thread, I'll read it then ;)

---

_Comment by @jessegrosjean on 2018-11-07 14:46_

I've implemented an ordered tree walk, originally based off the ignore create. I'm just using internally, it would require quite a bit of work to get back to ignore api state and feature set, but it might serve as a good reference point for evaluating how much performance and complexity overhead an ordered walk would take.

These times are all for walking over [linux checkout](https://github.com/BurntSushi/linux.git) with the configuration `.standard_filters(false).hidden(true).threads(8).build_parallel()`. My ordered walk implementation doesn't support pattern based filtering at the moment.

```
baseline_ignore_crate_walk_parallel: [67.141 ms 67.903 ms 68.274 ms]                              
```

In my ordered walk there are two kinds of order. The initial processing happens in "mostly" order. Each worker maintains a priority queue of work (ordered by index_path or each entry) . Workers exchange work items occasionally to keep queues balanced. 

```
ordered_walk_visit_all_entries_in_mostly_order: [77.634 ms 78.541 ms 80.383 ms]
```

On top of that "mostly" order you can apply a complete ordering. This works by gathering the results from each worker into another priority queue, and pulling the final results off the top when the top equals struct NextEntry.

```
ordered_walk::first_entry: [1.3928 ms 1.5433 ms 1.8609 ms]                             
ordered_walk::first_100_entries: [7.9247 ms 8.1353 ms 8.4698 ms]                                  
ordered_walk::first_1000_entries: [13.923 ms 14.904 ms 16.400 ms]                                                                          
ordered_walk::all_entries:  [131.08 ms 131.69 ms 132.38 ms]                       
```

Generally this all seems to be working to my liking. The mostly ordered processing doesn't add much overhead. The complete ordering does certainly add some overhead, but it also saves a lot of time if you are only interested in first_n entires as would be the case in my UI's.

Probably lots of room for improvement. This is my first big chunk of rust code, and also my first code that does more than just split up and process parallel processing. I'm sharing this code for anyone to do what they like with it. I would love to see the ignore crate get this type of complete ordering, but with parallel processing.

https://gist.github.com/jessegrosjean/6b3ca3a52249b65f58294dfafca12c91

---

_Comment by @BurntSushi on 2018-11-07 14:47_

@jessegrosjean Lovely! Thanks for sharing!

---

_Comment by @jessegrosjean on 2018-11-07 19:31_

I just posted an update to that gist that improves performance and simplifies things.

Previously work was shared between workers through a single channel. Also workers would occasionally "balance" their local work queue by pulling values out of this channel (if they needed work), or pushing into it (if they had too much).

The updated gist instead creates a separate channel for each worker. And each worker now gets a Vec of the sender half of all these channels. Now for each new work item the worker scheduling the item just sends it randomly to one of the other workers.

This is simpler and faster since there's no longer any need for "balance" work queue logic.

Now I see only 3-4ms difference between ignore's implementation and the ordered_walk_visit_all_entries_in_mostly_order test case.



---

_Comment by @jessegrosjean on 2018-11-08 00:38_

Ok, I said I was learning... I keep noticing places where my ordered walk needs fixing. After last update I realized that my version wasn't really doing work stealing. A work item that took a long time to process could block other work items (in that workers queue) even when other workers needed work.  

To solve this I now share all work queues with all workers. And when a workers queue is empty it will now try to steal work from another workers queue. This means all queues now need to be accessed through a mutex, but performance still seems really good. Stealing doesn't happen very often, so not much contention.

I also realized that in doing my benchmarks I was doing some extra work specific to my use. The end result is that the current version of the gist is performing a little better (by a few ms)  than the default ignore crate.

Summary is that (unless I'm missing something, possible!) performance shouldn't be an issue. This ordered walk is just as fast or faster. Here I'm talking about the underlying mostly ordered performance. Applying a total order (using `entries` method) does cost more (about double).

I'm going to leave this be for now, but let me know if you've got any questions.

---

_Comment by @BurntSushi on 2018-11-08 00:43_

Thanks for the writeup! What does memory usage look like? I think the
current parallel implementation is actually using more memory than it
should be, so I would be interested in both theoretical and actual answers,
if that makes sense. :)

On Wed, Nov 7, 2018, 19:38 Jesse Grosjean <notifications@github.com wrote:

> Ok, I said I was learning... I keep noticing places where my ordered walk
> needs fixing. After last update I realized that my version wasn't really
> doing work stealing. A work item that took a long time to process could
> block other work items (in that workers queue) even when other workers
> needed work.
>
> To solve this I now share all work queues with all workers. And when a
> workers queue is empty it will now try to steal work from another workers
> queue. This means all queues now need to be accessed through a mutex, but
> performance still seems really good. Stealing doesn't happen very often, so
> not much contention.
>
> I also realized that in doing my benchmarks I was doing some extra work
> specific to my use. The end result is that the current version of the gist
> is performing a little better (by a few ms) than the default ignore crate.
>
> Summary is that (unless I'm missing something, possible!) performance
> shouldn't be an issue. This ordered walk is just as fast or faster. Here
> I'm talking about the underlying mostly ordered performance. Applying a
> total order (using entries method) does cost more (about double).
>
> I'm going to leave this be for now, but let me know if you've got any
> questions.
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/152#issuecomment-436830483>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34mdEzBcEs5uZL0URjJ9hIfNHjMypks5us30OgaJpZM4KPYye>
> .
>


---

_Comment by @jessegrosjean on 2018-11-08 01:58_

Is there a good way to measure memory usage from within rust? Or should I use external tool?

For theoretical usage, let me see. I'm mostly making this up... mostly I just build my app and if it doesn't explode I call it NP good.

My understanding is that the way the current ignore works is for each WalkEntry encountered it pushes it into a channel. When a worker needs work it pulls off that channel.

I "think" ordered walk is doing pretty much the same thing in terms of overall memory use.

When ordered walk encounters a new WalkEntry it is also pushed into a channel. The difference is that instead of a single channel each worker has it's own channel. Each WalkEntry gets pushed into a randomly selected worker's channel. So instead of a single big channel, there are N smaller channels. And then each worker transfers the contents of their channel to a priority queue. Besides the overhead of extra channels, priority queues, etc... I don't think the actually number of entries alive in memory at once changes between the two implementations. When a worker needs work and it's local queue is empty it will just steal from another worker. So I think the total number of in flight entries is the same.

>  I think the current parallel implementation is actually using more memory than it should be

I'm not sure... I guess memory grows when you process a folder, since it generates more entries. So if you wanted to use less memory in the current implementation you could prioritize processing of files over processing of folders. That would keep the number of entries buffered in the channel smaller I think.

If that's actually true, then I guess the ordered walk code might help to actually lower memory usage. Instead of sorting alphabetically as the current code does you could sort files ahead of folders. And I think that might theoretically use less memory. Though it also might slow things down as you would be more likely to run into empty queue cases and be forced to try work stealing.

Maybe.

---

_Comment by @bmalehorn on 2018-11-08 03:15_

Figured I should comment here since I've messed around with file order and memory usage.

@BurntSushi you're right that there are memory problems in the current implementation. Since ripgrep uses a queue for searching files, it does something close to a BFS search. BFS uses memory proportional to the widest part of the tree. How wide is the widest part of your average source tree? E.g. how many files are 3 directories deep? Quite a few; I think Linux can get up to 10k elements in ripgrep's search queue.

In #450 I changed the work queue into a work stack, changing from BFS-ish to DFS-ish. It reduced memory usage from 8.8 MB to about 16 MB, since memory usage was now proportional to the max depth. It didn't get merged because of other reasons (mucking with the the sort order).

When you use a priority queue for the work, it will do something close to DFS. "Sorted order of files" is just a DFS walk over the files. So @jessegrosjean's patches will probably use less memory. Jesse if you want to test peak memory usage and you're on Linux, you can run `/usr/bin/time -f '%M' rg` which will output peak memory in kilobytes.

----

tl;dr: ripgrep does BFS, priority queues do DFS, BFS uses more memory than DFS

---

_Comment by @jessegrosjean on 2018-11-08 16:12_

My implementation could be made much simpler if there was a fast priority queue that the workers could all share, just as they share a single channel in the current implementation. Is there such a thing in rust? A priority channel?

Right now the only reason that each worker has own work queue is because if I use a single shared queue wrapped in a mutex there's to much contention and it's slow.

---

_Comment by @BurntSushi on 2018-11-08 16:14_

FWIW, I would try to find a way to use [rayon](https://docs.rs/rayon). It's possibly a research task though. I don't know how well it fits.

---

_Comment by @sharkdp on 2018-11-08 20:07_

@BurntSushi I have recently read through the source code in the `ignore` crate and found your comment regarding a possible parallel-walk implementation using `rayon`.

I used this as a motivation to try and implement my own simple parallel-walk implementation for a disk-usage-counting tool (which does not need the `gitignore`-features of `ignore`). The code for the rayon-based parallel walk can be found here:

https://github.com/sharkdp/diskus/blob/v0.4.0/src/walk.rs
(the interesting part is the `for_each_with` call that passes the `Sender` side of a channel as a first argument)

I am rather sure that this is *NOT* an ideal solution, since this was the first time that I have worked with rayon.

Still, the rayon-based implementation is a bit faster than the `ignore`-based version* (10% on a cold disk cache, 40% on a warm disk cache - see https://github.com/sharkdp/diskus/pull/15).

One thing that I don't like about my current implementation is that I need to copy all Paths into a `Vec` first before making the recursive call. As I understand it, this is due to the fact that the [`ReadDir`](https://doc.rust-lang.org/std/fs/struct.ReadDir.html) iterator is inherently sequential (due to the the usage of `readdir_r`).

(obviously my rayon-based implementation does not fulfill any ordering requirements, so I'm sorry if this is not really of any use here).

\* This might have other reasons too. I have noticed that the `ignore` crate still tries to find `gitignore` files like `.git/info/exclude` even though all ignore-related functionality is switched off.

Edit: Peak memory usage is a factor of four smaller for the `rayon` based version.

---

_Comment by @BurntSushi on 2019-03-27 12:13_

@sharkdp Somehow I missed your comment! Just seeing it now. Just wanted to chime in and thank you. Hopefully some day I'll be able to find time to dig back into this and look more closely and your work and @jessegrosjean's work, and see about rolling it back into `ignore`.

---

_Comment by @dgutov on 2020-12-10 21:06_

For anybody who stumbles on this discussion by accident, piping the result through `sort` works very well, and much more performant than `--sort path` in all the cases I've tried. Though perhaps, of course, it will change if the output becomes long enough (like, megabytes of text). But then, I'm not sure such searches are actually useful.

Example:

```sh
rg -e foo -nH file1 file2 file3 | sort -t: -k1,1 -k2n,2
```

The main downside is you don't see the results until the search completes, of course.

---

_Comment by @theimpostor on 2021-06-23 14:44_

@dgutov that's a great example of sorting the output and very fast indeed. I found that `sort` on my macbook does not implement the numeric sort of line numbers properly (the `-k2n,2` arg), but it does support stable sort (`-s`/`--stable`) which preserves the order of input when keys are identical, which seems to work just as well. Thus I have defined the following in my .bashrc:

```
function rgs {
    rg --line-number --with-filename --color always "$@" | sort --stable --field-separator=: --key=1,1
}
```


---

_Comment by @dgutov on 2021-06-23 16:36_

@theimpostor The state of command line tools on Apple systems is really bad (another instance of that I've heard of recently is broken behavior of the ancient version of `find` they suppy).

Is there a possibility to install newer `sort` through Homebrew, for example?

---

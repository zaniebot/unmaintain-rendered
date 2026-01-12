```yaml
number: 458
title: Over time the rg processes use more and more memory
type: issue
state: closed
author: setharnold
labels: []
assignees: []
created_at: 2017-04-24T00:12:20Z
updated_at: 2017-05-08T21:34:05Z
url: https://github.com/BurntSushi/ripgrep/issues/458
synced_at: 2026-01-12T16:13:22Z
```

# Over time the rg processes use more and more memory

---

_@setharnold_

Hello; I noticed that rg seems to use more and more memory over time. All the threads started out with less than two gigabytes in VIRT and RES columns in top, but after thirty minutes of wall-clock time, all the threads are above three gigabytes.

```
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                        
15911 sarnold   20   0    7.3m   2.2m   1.9m D 10.2  0.0   0:33.33 du                                             
15841 sarnold   20   0 3147.2m 2.984g   3.7m D  2.3  2.4   0:27.50 rg                                             
15854 sarnold   20   0 3147.2m 2.984g   3.7m D  2.3  2.4   0:27.64 rg                                             
15869 sarnold   20   0 3147.2m 2.984g   3.7m D  2.3  2.4   0:27.38 rg                                             
15846 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:27.50 rg                                             
15849 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:27.19 rg                                             
15856 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:27.10 rg                                             
15860 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:27.54 rg                                             
15861 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:27.11 rg                                             
15867 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:27.32 rg                                             
15871 sarnold   20   0 3147.2m 2.984g   3.7m D  2.0  2.4   0:26.76 rg                                             
15834 sarnold   20   0   41.9m   4.7m   3.0m R  1.6  0.0   0:20.33 top                                            
15842 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:27.42 rg                                             
15847 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:27.41 rg                                             
15848 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:26.91 rg                                             
15852 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:27.03 rg                                             
15857 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:26.95 rg                                             
15863 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:27.34 rg                                             
15864 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:27.30 rg                                             
15865 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:27.14 rg                                             
15870 sarnold   20   0 3147.2m 2.984g   3.7m D  1.6  2.4   0:26.79 rg                                             
15840 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.54 rg                                             
15843 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.34 rg                                             
15844 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.27 rg                                             
15845 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:26.97 rg                                             
15850 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.78 rg                                             
15851 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.50 rg                                             
15853 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.79 rg                                             
15855 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.56 rg                                             
15858 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.58 rg                                             
15859 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.00 rg                                             
15862 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:27.43 rg                                             
15866 sarnold   20   0 3147.2m 2.984g   3.7m D  1.3  2.4   0:28.04 rg                         
```

and then several minutes later:

```
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                        
15911 sarnold   20   0    7.4m   2.2m   1.9m D  8.9  0.0   1:03.98 du                                             
15852 sarnold   20   0 3347.2m 3.184g   3.7m D  3.0  2.5   0:31.66 rg                                             
15861 sarnold   20   0 3347.2m 3.184g   3.7m D  3.0  2.5   0:31.90 rg                                             
15842 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.92 rg                                             
15844 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.82 rg                                             
15848 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.50 rg                                             
15849 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.81 rg                                             
15855 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:32.03 rg                                             
15856 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.69 rg                                             
15859 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.56 rg                                             
15862 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:32.10 rg                                             
15868 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:31.71 rg                                             
15869 sarnold   20   0 3347.2m 3.184g   3.7m D  2.3  2.5   0:32.06 rg                                             
15841 sarnold   20   0 3347.2m 3.184g   3.7m D  2.0  2.5   0:32.07 rg                                             
15847 sarnold   20   0 3347.2m 3.184g   3.7m D  2.0  2.5   0:31.74 rg                                             
15850 sarnold   20   0 3347.2m 3.184g   3.7m D  2.0  2.5   0:32.41 rg                                             
15853 sarnold   20   0 3347.2m 3.184g   3.7m D  2.0  2.5   0:32.24 rg                                             
15864 sarnold   20   0 3347.2m 3.184g   3.7m D  2.0  2.5   0:31.99 rg                                             
15843 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:31.96 rg                                             
15845 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:31.60 rg                                             
15851 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:31.97 rg                                             
15854 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:32.40 rg                                             
15857 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:31.77 rg                                             
15858 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:32.32 rg                                             
15860 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:31.97 rg                                             
15867 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:32.06 rg                                             
15870 sarnold   20   0 3347.2m 3.184g   3.7m R  1.6  2.5   0:31.27 rg                                             
15871 sarnold   20   0 3347.2m 3.184g   3.7m D  1.6  2.5   0:31.38 rg                                             
15840 sarnold   20   0 3347.2m 3.184g   3.7m D  1.3  2.5   0:32.28 rg                                             
15863 sarnold   20   0 3347.2m 3.184g   3.7m D  1.3  2.5   0:31.74 rg                                             
15865 sarnold   20   0 3347.2m 3.184g   3.7m D  1.3  2.5   0:31.77 rg                                             
15834 sarnold   20   0   41.9m   4.7m   3.0m R  1.0  0.0   0:23.40 top                                            
15846 sarnold   20   0 3347.2m 3.184g   3.7m D  1.0  2.5   0:31.93 rg                                             
15866 sarnold   20   0 3347.2m 3.184g   3.7m D  1.0  2.5   0:32.53 rg                                             
```

Is this enough to worry about?

Thanks

---

_Comment by @BurntSushi on 2017-04-24 00:34_

I'm sorry, but this isn't enough data for me to give a helpful response. What command are you running? What version of ripgrep are you using? Can you reproduce this behavior on a public repository so that I can try reproducing it? If not, can you isolate the behavior to a single file? Do other programs, like `grep`, have the same behavior when running equivalent commands?

---

_Comment by @setharnold on 2017-04-24 01:01_

Sorry.

The command is: `rg -j 32 "hello world"`

I installed ripgrep via `cargo install ripgrep`; `cargo install --list` reports `ripgrep v0.5.1`

I can't give access to the machine in question but the dataset is public: the 'main' portion of Ubuntu source packages, unpacked; it's around 470GB of data in what I think is around 22M files and directories. 

The data being searched is roughly the unpacked contents of the main packages from Ubuntu.

Grep's memory use at start:

```
17316 sarnold   20   0   14.1m   2.5m   2.1m D  5.6  0.0   0:03.91 grep                                           
```

and after only five minutes:

```
17316 sarnold   20   0   14.4m   3.0m   2.2m D  1.3  0.0   0:10.43 grep                                           
```

I expect grep to still be running when I return, if it is I'll grab another line of its memory use at that point.

I can also give a try at reproducing this on smaller portions of the search space.

Thanks

---

_Comment by @BurntSushi on 2017-04-24 01:17_

> I can't give access to the machine in question but the dataset is public: the 'main' portion of Ubuntu source packages, unpacked; it's around 470GB of data in what I think is around 22M files and directories.

Can you teach me how to get that data on to a box? You may assume that I can spin up an Ubuntu 16.04 box on AWS. :-)

> I expect grep to still be running when I return, if it is I'll grab another line of its memory use at that point.

> I can also give a try at reproducing this on smaller portions of the search space.

Sorry, but can you also share the `grep` command you're using? And can you take that command, do `s/grep/rg`, run that, and check its memory use? The key here is to do a one-for-one comparison. If you're using `find | xargs grep`, then you should also try `find | xargs rg -j1`, for example. The `-j1` flag is important, since it puts ripgrep into single threaded mode and avoids unbounded output buffer growth.

---

Here's me thinking off the cuff here:

Since ripgrep runs in parallel, it has to buffer the entire output of searching a single file in memory. These buffers are reused, but they are never "shrunk." So if a search for `hello world` caused a significant number of results to appear from a single file, then it's possible the output buffers grew very large.

Another possibility is that there is a bug in the *input* buffer handling, where it found a file that didn't trip its binary file detection but otherwise contains a very long line. As with output buffers, input buffers are reused and never shrunk.

---

_Comment by @setharnold on 2017-04-24 07:09_

Sadly `rg` finished while I was away; `grep -r "hello world"` is still running:

```
17316 sarnold   20   0  119.6m 108.1m   2.2m D  5.9  0.1  22:30.33 grep
```

So grep went from 14M to 120M. Maybe rg going from 2gigs to 4gigs is no big deal.

Just for kicks here's the `time rg "hello world"` output:

```
real	151m47.449s
user	9m31.520s
sys	76m3.020s
```

I don't have the easiest mechanism to get just the Ubuntu sources unpacked; worse yet, I did the work six months ago and didn't take notes.

This will be insanely wasteful -- it copies EVERYTHING from the archives. That's fine for my uses but needless for this experiment:

- Mirror all the things:

```
#!/bin/bash
rsync --recursive --times --links --hard-links \
  --stats \
  --exclude "Packages*" --exclude "Sources*" \
  --exclude "Release*" --exclude "InRelease" \
  --exclude "Translation*" --exclude "Index" \
  rsync://archive.ubuntu.com/ubuntu /srv/mirror/ubuntu

rsync --recursive --times --links --hard-links \
  --stats --delete --delete-after \
  rsync://archive.ubuntu.com/ubuntu /srv/mirror/ubuntu
```

(This could probably be reduced to copying only the 'ubuntu/pool/main/' tree and probably --exclude "*deb" to get only sources, but I haven't tested these changes. There are local archives in S3 but I doubt they are rsync targets; if you want to try those, maybe the 'debmirror' tool may be a better starting point.)

- Unpack all the things:
I'm guessing at the command line I would have used; something like:

```
mkdir -p /srv/trees/ubuntu/
cd /srv/trees/ubuntu

for f in /srv/mirror/ubuntu/pool/main/*/*/*.dsc ; do g=${f#/srv/mirror/ubuntu/pool/} ; dpkg-source -x $f ${g%.dsc} ; done
```

The idea is to turn absolute filenames like `/srv/mirror/ubuntu/pool/main/a/axis/axis_1.4-16.dsc` into relative directory names for the dpkg-source -x command like `main/a/axis/axis_1.4-16`

(I haven't tested this on full-blown data but it should be close. Six months ago I used task-spooler to run multiple jobs at once; maybe gnu parallel would do the job easier.)

- Search all the things

```
cd /srv/trees/ubuntu/main/
rg -j 32 "hello world"
```

The full Ubuntu archive takes roughly 1.1 TB on my machine; the unpacked sources for 'main' here are roughly 470 GB.

Thanks

---

_Comment by @BurntSushi on 2017-04-24 10:52_

Hmm, yes... If `grep` is using ~100MB and ripgrep is running 32 threads, then `32 * 100MB = ~3GB` seems like reasonable memory usage. I feel like I'm reading tea leaves a bit here, but it sounds like this is probably connected to some set of files that cause the input buffer to grow large. If that's true, then I'd guess that `rg -j1` would have similar memory usage as `grep`.

I think it would be interesting to know exactly which types of files cause this, since ideally, the input buffer stays relatively small. IIRC, the input buffer grows until it fits at least one line in memory. So if you have a 100MB JSON file (for example) that is all on one line, then that could cause the spike.

I'd still like to take a look at this, but your result from `grep` significantly lowers the priority for me.

---

_Comment by @lespea on 2017-04-24 14:45_

After each file has been processed, would it be possible to just look at how big the buffer is before you reuse it and if it's really large, maybe just shrink it down first?

---

_Comment by @BurntSushi on 2017-04-24 15:09_

@lespea Well... sure. But step 1 is figuring out the problem. And that solution isn't universally good. If every file in the corpus requires a large buffer, then you end up doing needless allocation thrashing.

---

_Comment by @BurntSushi on 2017-05-08 21:34_

I'm going to close this. I think the fact that grep uses a similar amount of memory (proportional to the number of threads) means that there probably isn't anything seriously wrong here. I do think it would be fun to take at the corpus and get a clearer understanding of where memory is going, but if I'm going to be realistic, I don't think I'll be downloading 470GB to an AWS server any time soon.

---

_Closed by @BurntSushi on 2017-05-08 21:34_

---

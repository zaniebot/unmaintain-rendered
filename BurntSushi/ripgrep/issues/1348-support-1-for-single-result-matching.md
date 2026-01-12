```yaml
number: 1348
title: "Support `-1` for single-result matching"
type: issue
state: open
author: alexmv
labels:
  - question
assignees: []
created_at: 2019-08-14T03:48:41Z
updated_at: 2025-09-16T11:30:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1348
synced_at: 2026-01-12T16:13:23Z
```

# Support `-1` for single-result matching

---

_@alexmv_

#### What version of ripgrep are you using?

```
mycon ~ $ rg --version
ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
-SIMD -AVX (runtime)
```

#### How did you install ripgrep?

apt package from Releases.

#### What operating system are you using ripgrep on?

```
mycon ~ $ lsb_release -d
Description:	Ubuntu 16.04.5 LTS
```

#### Describe your question, feature request, or bug.

Searching in a large number of files for the only file that can match is may be slower than it needs to be.  As an example, my inciting use case is looking for the location of a mail message in a reasonably-sized (9G, 400k files) Maildir, given the `Message-Id`.  In this case, only one file will possibly match; while `-m 1` limits to one result per file, no flag exists to abort after the first overall result.  Unfortunately, adding `| head -n1` does not help because no further output is seen from `rg` to send it the SIGPIPE to abort:
```
mycon ~/Mail $ time rg -m 1 -l --hidden redacted-message-id
.Trash/cur/some-redacted-path

real	0m1.388s
user	0m3.662s
sys	0m6.730s
mycon ~/Mail $ time rg -m 1 -l --hidden redacted-message-id | head -n1
.Trash/cur/some-redacted-path

real	0m1.467s
user	0m3.556s
sys	0m6.871s
```

The 1.4s is after loading everything into caches; before doing so, searching took more than 3 minutes, despite the result being found in the first 0.13s:
```
mycon ~/Mail $ time rg -m 1 -l --hidden redacted-message-id | ts -s %.s
0.126295 .Trash/cur/some-redacted-path

real	3m8.019s
user	0m4.593s
sys	0m9.705s
```

`ack` uses the `-1` flag for this, which is useful, particularly in conjunction with `-l`.  This finds results an order of magnitude faster than empty-caches `rg` -- but an order of magnitude slower than an `rg` which could quick-abort:
```
mycon ~/Mail $ time ack -1 -l redacted-message-id
.Trash/cur/some-redacted-path

real	0m3.872s
user	0m3.328s
sys	0m0.562s
```

---

_Comment by @BurntSushi on 2019-08-14 10:33_

Folks have proposed this feature (or something like it) in the past, and I've historically been against it. In particular, the order in which files are searched is not only not specified, but it is technically non-deterministic. It is non-deterministic even when ripgrep is single threaded, although in practice, file systems generally return a consistent ordering across multiple calls. The only way to cause ripgrep's output to be deterministic is to use `--sort path`, but this disables parallelism and incurs the overhead of sorting directory entries as they are traversed.

Because of the non-determinism, the fundamental problem with "only show me the top N results" is that the top N results can _change_ from run to run. It's quite misleading and can, I imagine, result in non-obvious bugs where folks might assume that the top N results don't change.

Now, your case is a little different since you know there is exactly one match. So the search can stop as soon as it's found and the output will always be the same from run to run. However, still, the fact that there is a noticeable performance improvement by limiting oneself to a single match is pretty much nothing but luck. ripgrep does not guarantee the order in which it searches files, so it could very well be that your match isn't found until ripgrep searches the very last file. You could make a compelling probabilistic argument here by saying that the odds of the last file (or even near the last file) being searched containing the match are fairly rare.

So basically, in summary, my reservations are the following:

* Adding a flag like this might give folks the false impression that the top N results are invariant given the same inputs, but they are not.
* The performance improvements you desire are effectively down to chance, since it depends on _when_ ripgrep searches the file.
* I'm hesitant about allocating `-1` for this, since short flags are in short supply. Perhaps `--max-hits` would be more appropriate here.

---

_Label `question` added by @BurntSushi on 2019-08-14 10:33_

---

_Comment by @alexmv on 2019-08-14 17:25_

> Adding a flag like this might give folks the false impression that the top N results are invariant given the same inputs, but they are not.

Yeah -- I added and then removed a comment to the effect of "this result may, of course, be non-deterministic" but I removed it because it seemed obvious, based on the fact that I'd observed that the result ordering was already non-deterministic.

Anyone who is _currently_ post-processing the results should be aware that the ordering is not invariant; this is currently lightly documented under `--sort` but maybe bears a little more prominence.

Phrasing as "stop after only only one result; this result may not be consistent unless used with `--sort`" seems like it adequately documents this.

> The performance improvements you desire are effectively down to chance, since it depends on _when_ ripgrep searches the file.

Sure.  The expectation is, of course, 50% faster -- though in many cases might be better than that, if it's run multiple times and gets the advantage of finding a result while still looking in data loaded from disk caches.

> I'm hesitant about allocating `-1` for this, since short flags are in short supply. Perhaps `--max-hits` would be more appropriate here.

I suggested `-1` because of the prior art, but I'm in no way wedded to it.  I guess I was assuming that "exit after one result" was slightly easier to imply the non-determinism on than the more general "exit after /N/ results" -- but the greater generality potentially outweighs the increased clarity, yes.

`--max-hits` doesn't seem to contrast well with `--max-count`, in that it's not clear from their names which is per-file, and which is overall.  `--total-count` ?  `--i-accept-non-determinism-top-n` ?

---

_Comment by @BurntSushi on 2019-08-14 17:41_

Yeah, point taken about the flag name. I did want to still try and keep it short though. `--total-count` is serviceable. Anything shorter?

I _think_ you might have sold me on this. I'll keep noodling. I _think_ this should be straight-forward to implement. Mostly plumbing. There might be some complexity in handling this correctly in the parallel searcher. I'd guess you'd want to [return `WalkState::Quit`](https://github.com/BurntSushi/ripgrep/blob/ef0e7af56a9b1afce7be46db87b3ff3709187ee2/src/main.rs#L169) here, but you have to be a little careful not to race with other threads.

Also, `--total-count` (or whatever we pick) also need to _imply_ `--max-count` with the same value too.

And now I've realized a complication. If you request `--total-count 5`, and you search two files, one with `4` matches and another with `3` matches, then it's pretty tricky to print _just_ `5` of those matches. We might need to settle for "`--total-count` _approximately_ limits the total number of matches to `N`."

---

_Comment by @alexmv on 2019-08-14 17:51_

I can't come up with anything shorter off the top of my head, especially without renaming `--max-count`, which isn't possible.

Mmm.  Does the printer know how many records it has output, in the stats object?  Can the searcher return approximately the right number of results, and instead enforce a stricter limitation during output, which is probably not parallel?  I've only briefly kicked around in the code, so take with an appropriate grain of salt.

---

_Comment by @BurntSushi on 2019-08-14 19:39_

The printer will tell you the number of matches, yes. The problem is that the printer writes its output as-is and there's no easy way to cut that off after it has been written. You could re-configure the printer to only print `k` matches (where `k = N - already-printed`), but there's still a race.

The only thing that isn't parallel is dumping the output of the printer to `stdout`. By the time you get there, the output has already been prepared in an in-memory buffer.

(Getting this fully correct for single threaded search is possible though.)

---

_Comment by @alexmv on 2019-08-14 20:07_

Hm, got it.  Just saying "_approximately_ limits" seems a little weasel-wordy -- and it seems worth calling out somehow that using `--sort file` will make the result both stable and correct, at the cost of parallelism.  Maybe something like:

> `--total-count` attempts to limit the total number of results output; contrast to `--max-count`, which is a per-file limit.  This is best-effort; ripgrep may output more than the requested number of results if parallelism is used (the default; see `-j`).  Which results are output is also non-deterministic unless `--sort` is used (which also disables parallelism).

Writing it out like that, it does seem like a bucket of caveats, though.

---

_Comment by @genivia-inc on 2020-01-20 20:19_

> > I'm hesitant about allocating `-1` for this, since short flags are in short supply. Perhaps `--max-hits` would be more appropriate here.
> 
> I suggested `-1` because of the prior art, but I'm in no way wedded to it. I guess I was assuming that "exit after one result" was slightly easier to imply the non-determinism on than the more general "exit after /N/ results" -- but the greater generality potentially outweighs the increased clarity, yes.

My 2c from "lessons learned": as you know there are two commonly-referenced "standards" with respect to command-line options:

1. [POSIX](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) permits numeric (one digit) command-line options 
2. [GNU getopt](https://www.gnu.org/software/libc/manual/html_node/Getopt-Long-Options.html) also considers numeric options as valid and other characters as a GNU extension, although legacy usage is restricted to letters only.

On the other hand, consider this: concatenated options may cause ambiguity, as for example `-a1` could mean `-a` with the value 1 or the two options `-a -1` combined. Or worse, `-a10` which could be `-a` with the value 10 or the three options `-a -1 -0` combined. That's why numeric short options are uncommon and perhaps should be avoided in practice and were never used by legacy unix utilities.

> `--max-hits` doesn't seem to contrast well with `--max-count`, in that it's not clear from their names which is per-file, and which is overall. `--total-count` ? `--i-accept-non-determinism-top-n` ?

What about `--max-files=1` combined with `-m1` to produce only one hit for the first file found?
~~~
-m NUM, --max-count=NUM
        Stop reading the input after NUM matches for each file processed.
--max-files=NUM
        If -R or -r is specified, restrict the number of files matched to
        NUM.  Specify -J1 to produce replicable results by ensuring that
        files are searched in the same order as specified.
~~~
 This is [documented and implemented in ugrep](https://github.com/Genivia/ugrep#max). I think the caveat included in the description is a fair warning (`-J1` sequentializes the search), as it refers to "the same order as specified", leaving it up to the order of the command line arguments and the order in which the OS lists directory contents.

---

_Comment by @hustcer on 2023-08-12 12:33_

+1 for this feature

---

_Comment by @xaocon on 2024-02-22 21:29_

Maybe the arg could be `--first` which wouldn't take an argument instead? It's less flexible but maybe gets the point across better?   It would be great to have some kind of option to short circuit the search when you need a single piece of info.

---

_Comment by @wshanks on 2024-03-13 15:46_

Here is a slightly different use case from the original suggestion of knowing ahead of time that there is exactly one match: I would like to search a series of directories to classify them based on whether or not a certain pattern occurs within them. I don't need deep analysis of where and how that pattern is used (maybe there is a different tool I should be using). `--first` would be a good fit for the name for this use case.

---

_Comment by @giladbarnea on 2024-10-18 18:15_

Would be great to have this feature

---

_Comment by @buergi on 2025-09-16 11:26_

Just missed this as well, especially to only output the first match.
Update: Ah sorry, this is targets a single result in multiple files. I was looking for first result in each file, which is exactly what `--max-count` does, as far as I understand it.

---

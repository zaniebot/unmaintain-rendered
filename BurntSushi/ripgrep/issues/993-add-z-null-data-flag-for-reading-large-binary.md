```yaml
number: 993
title: add -z/--null-data flag for reading large binary files
type: issue
state: closed
author: Ekleog
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2018-07-25T17:40:03Z
updated_at: 2020-03-07T00:20:03Z
url: https://github.com/BurntSushi/ripgrep/issues/993
synced_at: 2026-01-12T16:13:22Z
```

# add -z/--null-data flag for reading large binary files

---

_@Ekleog_

#### What version of ripgrep are you using?

Version 0.8.1 [as built by NixOS](https://nixos.org/nixos/packages.html#ripgrep)

#### Describe your question, feature request, or bug.

Hmm, so I think I just hit an issue similar (but not identical) to https://github.com/BurntSushi/ripgrep/issues/458, when running `rg [a string not on the disk] /dev/nvme0n1p3`. (this being a ~100G partition, on an 8G-RAM machine). The difference being that, here, rg gets oom-killed in a matter around 1min.

Dmesg output of the OOM killer http://xelpaste.net/1P14FN.

#### If this is a bug, what are the steps to reproduce the behavior?

Run `rg [some search pattern] [some big partition]`. That said I can't reproduce on the computer on which I'm typing this, which has `sda*` partitions, and for which search finishes in a matter of seconds, so there may be something special about `nvme0n1p*`?

#### If this is a bug, what is the actual behavior?

Ripgrep gets OOM-killed

#### If this is a bug, what is the expected behavior?

Either ripgrep could have managed to actually perform the search (the pattern was something very simple, no regex or anything, so afaiu theoretically the search could be done with very limited memory use in 3 passes for the output, forwards until the pattern, then backwards until a newline, then forwards again while outputting until the next newline), which would likely be best, but quite potentially require large changes to the codebase.

Or ripgrep could display an error message saying “Sorry we couldn't allocate enough memory because {your file is too big, your pattern is too complex, …}”, so that the reason why ripgrep is OOM'ing is clear. And if not possible, even a “OOM” message would still be better than the SIGKILL rg currently receives through the OOM.

Anyway, these are just nitpicks, thank you for ripgrep!

---

_Comment by @BurntSushi on 2018-07-25 17:50_

> so afaiu theoretically the search could be done with very limited memory use in 3 passes for the output, forwards until the pattern, then backwards until a newline, then forwards again while outputting until the next newline

This is wrong, because lines are variable length. This is one reason why ripgrep (and GNU grep) do binary data detection. If a NUL byte is found, then by default, ripgrep gives up, because binary data printed to terminals is bad but also because binary data doesn't have a newline distribution similar to plain text files, which can result in allocating a lot of memory.

When crawling a large directory, ripgrep should be using a fixed size buffer with standard `read` calls. There are only two ways that this buffer can increase beyond its default 8KB capacity: 1) it needs to make room for contextual lines, which only happens when the `-A/-B/-C` flags are used or 2) there is a line longer than 8KB.

With that said, ripgrep has other ways of using memory. For example, its parallel searcher may be too eager and fill up heap space with the file tree. Typically this is small enough that it doesn't matter, but you might consider using the `-j1` flag (to force single threaded mode) to see if that helps memory usage.

Another way ripgrep uses memory is for output. Namely, when using multithreading, ripgrep buffers the entire output of searching a file in memory before dumping it to stdout in order to prevent the interleaving of matches. So for example, if you're searching a file bigger than 8GB with a pattern that matches everything, then you're going to exhaust available memory.

> Or ripgrep could display an error message saying “Sorry we couldn't allocate enough memory because {your file is too big, your pattern is too complex, …}”, so that the reason why ripgrep is OOM'ing is clear. 

That's not how OOM conditions happen on Linux, and at the very minimum, depends on your system settings for overcommit. Moreover, your suggested error messages are a bit weird. Firstly, "file too big" isn't really related at all; what matters is its distribution of bytes, which can't be known until its read. So the only thing to do is to attempt to search it. Secondly, "your pattern is too complex" is already accounted for. You'll get an error if you try to search with a pattern that is too big (try `rg '\pL{100}{100}{100}'`).

I don't foresee ripgrep doing anything to make the OOM killer failure mode any better. By the time the OOM killer has ripgrep in its sights, it's too late. It is the user's responsibility to become familiar with the failure modes of OOM killers on systems with OOM killers enabled. Moreover, that ripgrep is the target of an OOM killer does not necessarily imply that ripgrep is the agitator. I note that no part of your issue mentions the observed peak memory usage of ripgrep, which is a critical piece of information when reporting a bug about an OOM condition.

If you don't have an OOM killer and your system allocators don't lie, then ripgrep should indeed exit with a more descriptive error.

Here are some things to try:

1. Use `-j1` to force single threaded mode.
2. Compare `--mmap` with `--no-mmap`.
3. Investigate the memory usage of GNU grep on the same directory.
4. Investigate the memory usage of ripgrep and GNU grep with the `-a` flag set.

---

_Label `question` added by @BurntSushi on 2018-07-25 17:56_

---

_Comment by @Ekleog on 2018-07-26 09:41_

On 07/26/2018 02:51 AM, Andrew Gallant wrote:
>> so afaiu theoretically the search could be done with very limited memory use in 3 passes for the output, forwards until the pattern, then backwards until a newline, then forwards again while outputting until the next newline
> 
> This is wrong, because lines are variable length. This is one reason why ripgrep (and GNU grep) do binary data detection. If a NUL byte is found, then by default, ripgrep gives up, because binary data printed to terminals is bad but also because binary data doesn't have a newline distribution similar to plain text files, which can result in allocating a lot of memory.

Well, even with lines of variable lengths, I'm pretty sure it's possible
to do the job in O(1) memory regardless of the file or contents of the
file. For instance, an algorithm (pseudo-code, obviously not optimized,
nor checked for its likely multiple bugs, just for the idea):

```
fn handle_file(f: File, r: Regex) {
    loop {
        // Find the next occurence of the pattern
        loop {
            if let Some(c) = read_1_char_from_file(f) {
                match r.feed_1_char_to_FSA(c) {
                    FoundPattern => break,
                    NotFoundYet => (),
                }
            } else {
                return;
            }
        }
        // Figure out the beginning of the line
        loop {
            if let Some(c) = read_1_char_backwards_from_file(f) {
                if c == '\n' {
                    break;
                }
            } else {
                break;
            }
        }
        // Output until the end of the line
        loop {
            if let Some(c) = read_1_char_from_file(f) {
                putchar(c);
                if c == '\n' {
                    break;
                }
            } else {
                break;
            }
        }
    }
}
```

This requires the ability to seek a file backwards, but for files on
disk this shouldn't be an issue? (apart from the added code complexity,
of course)

> When crawling a large directory, ripgrep should be using a fixed size buffer with standard `read` calls. There are only two ways that this buffer can increase beyond its default 8KB capacity: 1) it needs to make room for contextual lines, which only happens when the `-A/-B/-C` flags are used or 2) there is a line longer than 8KB.
> 
> With that said, ripgrep has other ways of using memory. For example, its parallel searcher may be too eager and fill up heap space with the file tree. Typically this is small enough that it doesn't matter, but you might consider using the `-j1` flag (to force single threaded mode) to see if that helps memory usage.

Here I'm grepping through a single 100G-long partition pseudo-file,
`/dev/nvme0n1p3`, so I guess that's not the issue

>> Or ripgrep could display an error message saying “Sorry we couldn't allocate enough memory because {your file is too big, your pattern is too complex, …}”, so that the reason why ripgrep is OOM'ing is clear. 
> 
> That's not how OOM conditions happen on Linux, and at the very minimum, depends on your system settings for overcommit.
> 
> I don't foresee ripgrep doing anything to make the OOM killer failure mode any better. By the time the OOM killer has ripgrep in its sights, it's too late. It is the user's responsibility to become familiar with the failure modes of OOM killers on systems with OOM killers enabled. Moreover, that ripgrep is the target of an OOM killer does not necessarily imply that ripgrep is the agitator. I note that no part of your issue mentions the observed peak memory usage of ripgrep, which is a critical piece of information when reporting a bug about an OOM condition.

Indeed, it was behind the paste to the dmesg output of the OOM killer,
sorry for not having thought of making it easier to see!

The line relevant to ripgrep there is:
```
uid  tgid total_vm      rss nr_ptes nr_pmds swapents oom_score_adj name
  0 30187  4206947  1573363    5151      14        0             0 rg
```

> If you don't have an OOM killer and your system allocators don't lie, then ripgrep should indeed exit with a more descriptive error.
> 
> Here are some things to try:
> 
> 1. Use `-j1` to force single threaded mode.
> 2. Compare `--mmap` with `--no-mmap`.
> 3. Investigate the memory usage of GNU grep on the same directory.
> 4. Investigate the memory usage of ripgrep and GNU grep with the `-a` flag set.

Oh sorry it looks like I forgot the `-a` in my previous message's command!

# Detailed analysis

The command run is searching for `qwertyuiop` on `/dev/nvme0n1p3` (ie. a
single 100G-long file). Each section below will be named
`exec-and-options`, which will correspond to a run of `exec-and-options
qwertyuipo /dev/nvme0n1p3`. Measurements made with the `psrecord` python
package.

## `grep`

Returns with no output. Memory and CPU usage: http://xelpaste.net/1uV0rQ

## `rg`

Returns with no output in less than 1 second, so no graph.

## `rg -a`

OOM-killed, displayed on console: “Killed”. http://xelpaste.net/u3nMpg

## `rg -j1 -a`

OOM-killed, displayed on console: “Killed”. http://xelpaste.net/69UhjH

BTW, I don't really get why it's still at ~200% CPU, but anyway.

## `rg --mmap -a`

OOM-killed, same message. http://xelpaste.net/Udv1jV

## `rg --no-mmap -a`

OOM-killed, same message. http://xelpaste.net/7dqK8w

## `grep -a`

OOM-killed, displayed on console “grep: memory exhausted”. Note I think
this is already a much better error message than “Killed” :)

http://xelpaste.net/NqHnr1

## `grep --only-matching --byte-offset --binary --text`

OOM-killed, same message. http://xelpaste.net/9L87gY

## `grep --only-matching --byte-offset --binary -z --text`

Works! But not reproducible with `rg` due to no `--byte-offset` or `-z`
parameter I could easily find.

http://xelpaste.net/uVToz4

## `grep --byte-offset --binary -z --text`

Works too, gives a bit of context. http://xelpaste.net/3Xqfiz

------

Given the fact that the memory allocation seems to be done by blocks of
4G (I guess the size of the line, for a line buffer), I guess the above
(slower due to being 3-pass) algorithm for using only a fixed amount of
memory could be triggered when trying to allocate more than eg. 1G of
memory? Either that, or just refusing to allocate a 1G line buffer
without a special flag, given it's quite sure that's not what the user
actually wants.

What do you think about that?


---

_Comment by @Ekleog on 2018-07-26 09:42_

Ugh. Looks like GitHub doesn't like Markdown by email. So some text is hidden under the “signature” marker, I'll paste it here for ease of reading:

Given the fact that the memory allocation seems to be done by blocks of 4G (I guess the size of the line, for a line buffer), I guess the above (slower due to being 3-pass) algorithm for using only a fixed amount of memory could be triggered when trying to allocate more than eg. 1G of memory? Either that, or just refusing to allocate a 1G line buffer without a special flag, given it's quite sure that's not what the user actually wants. What do you think about that?

---

_Comment by @BurntSushi on 2018-07-26 11:41_

> I'm pretty sure it's possible

Let me stop you right there and finish that thought:

"I'm pretty sure it's possible... after years of work and research to rewrite ripgrep and its regex engine from the ground up." By "research" I mean, "figure out how to efficiently use a finite state machine in streaming mode." Your example code doesn't cut it unfortunately. I don't know of any extant production grade regex engine that uses finite state machines and satisfies the criterion of being able to handle arbitrarily sized inputs in constant space while still meeting the requirements of ripgrep.

I have an extensive write up here: https://github.com/rust-lang/regex/issues/425#issuecomment-348768742

Can we drop the elementary lessons on finite state machines? Thanks. Let's please focus on the problem.

> Oh sorry it looks like I forgot the `-a` in my previous message's command!

Yes, that's critical. When the `-a` flag is enabled, no binary detection is performed, which means ripgrep's in-memory buffer will grow at least as big as the longest streak of bytes that do not contain a `\n`.

> BTW, I don't really get why it's still at ~200% CPU, but anyway.

Weird. `-j1` on my machine shows it is using 100%. This actually sounds like a bug, but probably unrelated to this issue. 

> OOM-killed, displayed on console “grep: memory exhausted”. Note I think
this is already a much better error message than “Killed” :)

I would agree, and I find the behavior difference here interesting. If GNU grep can detect a failed `malloc`, then I'd expect ripgrep to be able to do the same, and subsequently abort on its own. One possible difference is that GNU grep is using the system allocator while ripgrep uses jemalloc. I'm not sure off hand if that would make a difference.

With that said, the fact that GNU grep is exhausting memory here to me means that ripgrep is working as intended.

> `grep --only-matching --byte-offset --binary -z --text`
>
> Works! But not reproducible with `rg` due to no `--byte-offset` or `-z`
> parameter I could easily find.
>
> http://xelpaste.net/uVToz4
>
> `grep --byte-offset --binary -z --text`
>
> Works too, gives a bit of context. http://xelpaste.net/3Xqfiz

OK, now we're getting somewhere. This basically confirms my diagnosis. Namely, the `-z` flag is the key ingredient, because it effectively change's GNU grep's line terminator from `\n` to `\x00`. Given a raw data dump, the distribution of `\x00` is probably quite common, which means GNU grep _just happens_ to work well here. If you had a data dump where both `\n` and `\x00` were rare, then you'd be witnessing the same sort of memory exhaustion.

This is good news, because my work on libripgrep should make it much easier to support the same `-z/--null-data` flag that you're using in GNU grep, so I'll accept this as a bug that should hopefully get fixed when I pull in libripgrep.

I can't think of any simple work around until then unfortunately. (Other than, of course, just using grep, which is probably going to perform similarly as ripgrep for this sort of task given that it's assuredly I/O bound.)

Note that I suspect the `--byte-offset` and `--binary` flags are superfluous here. You should be able to just use `-z -a` to get a working grep.

---

_Renamed from "ripgrep OOM killed" to "add -z/--null-data flag for reading large binary files" by @BurntSushi on 2018-07-26 11:52_

---

_Label `enhancement` added by @BurntSushi on 2018-07-26 11:52_

---

_Label `libripgrep` added by @BurntSushi on 2018-07-26 11:52_

---

_Label `question` removed by @BurntSushi on 2018-07-26 11:52_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-07-26 11:52_

---

_Comment by @Ekleog on 2018-07-26 17:39_

>> I'm pretty sure it's possible
> 
> Let me stop you right there and finish that thought:
> 
> "I'm pretty sure it's possible... after years of work and research to rewrite ripgrep and its regex engine from the ground up." By "research" I mean, "figure out how to efficiently using a finite state machine in streaming mode." Your example code doesn't cut it unfortunately. I don't know of any extant production grade regex engine that uses finite state machines and satisfies the criterion of being able to handle arbitrarily sized inputs in constant space while still meeting the requirements of ripgrep.
> 
> I have an extensive write up here: https://github.com/rust-lang/regex/issues/425#issuecomment-348768742

I don't want to argue with that in the general case. However, for search
parameters as simple as the `qwertyuiop` given in this example, I'm
positive it can be done in 3-pass with a constant buffer, with the
stupid algorithm I gave above. Not saying the code I'd write would be as
fast as ripgrep's, but at least it would eventually give an output.

Actually, I'm almost sure that what I'm saying is also what you wrote in
your write-up.

Also, I'd like to mention that in my first post I already had the “which
would likely be best, but quite potentially require large changes to the
codebase”.

> Can we drop the elementary lessons on finite state machines? Thanks. Let's please focus on the problem.

Can we drop aggressiveness too? I must say I've felt like the first
three words of your first answer being “This is wrong” didn't set the
right tone, but I hoped it was due to my mis-using English in the top
post, given you also appeared to think I was searching in a big
directory while I thought I had written that it was on a partition file.
Now… well, let's say I'd hope we could talk in a friendly manner.

Anyway, I must say I likely won't answer again if I feel once more like
we're just misunderstanding each other and I'm just bothering you :)

>> BTW, I don't really get why it's still at ~200% CPU, but anyway.
> 
> Weird. `-j1` on my machine shows it is using 100%. This actually sounds like a bug, but probably unrelated to this issue. 

I thought of reporting it as a bug, but given it was reported by
`psrecord`, a software which I don't know at all, I thought it may just
as well be a bug of the tool. I'll try to have a look at htop next time
I get to that computer :)

>> OOM-killed, displayed on console “grep: memory exhausted”. Note I think
> this is already a much better error message than “Killed” :)
> 
> I would agree, and I find the behavior difference here interesting. If GNU grep can detect a failed `malloc`, then I'd expect ripgrep to be able to do the same, and subsequently abort on its own. One possible difference is that GNU grep is using the system allocator while ripgrep uses jemalloc. I'm not sure off hand if that would make a difference.
> 
> With that said, the fact that GNU grep is exhausting memory here to me means that ripgrep is working as intended.

Well, it depends on the goal of ripgrep. If it's to be “grep, but
faster”, then indeed. However, the readme also mentions “fewer bugs”,
and I'd argue handling OOM failures better than grep would be something
going in this direction.

By “handling OOM failures better than grep”, I'm thinking mostly of
dropping back to the slower regex engine when it could be detected that
the faster regex engine would likely trigger an OOM (eg. when it tries
to allocate >= 1GB RAM), if I understood correctly and your extensive
write-up mentioned that the slower regex engine was less memory-consuming.

>> `grep --only-matching --byte-offset --binary -z --text`
>>
>> Works! But not reproducible with `rg` due to no `--byte-offset` or `-z`
>> parameter I could easily find.
>>
>> http://xelpaste.net/uVToz4
>>
>> `grep --byte-offset --binary -z --text`
>>
>> Works too, gives a bit of context. http://xelpaste.net/3Xqfiz
> 
> OK, now we're getting somewhere. This basically confirms my diagnosis. Namely, the `-z` flag is the key ingredient, because it effectively change's GNU grep's line terminator from `\n` to `\x00`. Given a raw data dump, the distribution of `\x00` is probably quite common, which means GNU grep _just happens_ to work well here. If you had a data dump where both `\n` and `\x00` were rare, then you'd be witnessing the same sort of memory exhaustion.
> 
> This is good news, because my work on libripgrep should make it much easier to support the same `-z/--null-data` flag that you're using in GNU grep, so I'll accept this as a bug that should hopefully get fixed when I pull in libripgrep.
> 
> I can't think of any simple work around until then unfortunately. (Other than, of course, just using grep, which is probably going to perform similarly as ripgrep for this sort of task given that it's assuredly I/O bound.)
> 
> Note that I suspect the `--byte-offset` and `--binary` flags are superfluous here. You should be able to just use `-z -a` to get a working grep.

That's most likely indeed, the `--byte-offset` was useful for me to then
use `dd` and try to figure out what was around the pattern in the
partition image.

Anyway, supporting the `-z` flag sounds like a good work-around, even
though it isn't solving the underlying issue of memory consumption
(that, granted, would be really hard to solve) :)

Again, thank you for your work on ripgrep!


---

_Comment by @BurntSushi on 2018-07-26 19:41_

Apologies for the aggression, my frustration got the better of me. Your comments have not be easy to read or respond to. As such, I will try to be brief and to the point.

The `-z/--null-data` flag sounds like a good addition to ripgrep. This issue can track that feature.

I will not be spending my time improving the failure mode of OOM conditions. I agree that GNU grep's failure mode is better. I will not be beholden to nitpicks about particular wordings of project goals. (The word "goal" doesn't appear once in the project README, largely to avoid that style of argumentation, which I find _incredibly_ frustrating as a maintainer.) I would be happy if someone else wanted to improve this failure mode, so long as it didn't impose any significant complications on the code. I don't see any reason to track this particular issue.

It is technically true that some special cases could be implemented in constant space. We don't need to go over how this could be done in a simplistic scenario, because that's not what's interesting. What is interesting are the practical ramifications of implementing those special cases along with maintaining the general case implementation. This is why I jumped to the general case; if it can be solved then all searches can benefit regardless of whether they are special or not. As such, I will not be investing any of my time in the near future to fixing special cases. My long term goal is to _attempt_ to address this in the regex engine, if it's possible. If that effort succeeds, then it may bear fruit that ripgrep can benefit from. I don't see any reason to track this particular issue, as the next immediate work here comes in the regex engine, which is already being tracked.

---

_Comment by @BurntSushi on 2018-07-26 22:22_

I compiled a version of ripgrep that uses the system allocator, and my guess seems to be right: it yields a better error message.

With jemalloc (default):

```
[andrew@Leopard ripgrep] time ./target/release/rg-master Openbox /dev/sda -c -a -j1
Killed
real    1m15.251s
user    0m5.834s
sys     0m21.550s
```

With system allocator:

```
[andrew@Leopard ripgrep] time ./target/release/rg Openbox /dev/sda -c -a -j1
memory allocation of 8589934592 bytes failedAborted (core dumped)
real    1m26.059s
user    0m4.824s
sys     0m22.670s
```

It will be a while before ripgrep can move to the system allocator by default, but when that's possible, we might pursue that option. Playing around with it a little bit does actually seem to cause some small performance regressions in cases, but we can cross that bridge when we get there.

---

_Comment by @BurntSushi on 2018-07-26 23:10_

OK, I decided to do an experiment and actually look at what's going on in terms of allocation, since I wanted to know whether moving to the system allocator would actually be a plausible solution to this problem. The short answer is, no, it wouldn't be.

Here is the strace with the system allocator:

```
mremap(0x7f8c54b98000, 4294971392, 8589938688, MREMAP_MAYMOVE) = -1 ENOMEM (Cannot allocate memory)
mmap(NULL, 8589938688, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = -1 ENOMEM (Cannot allocate memory)
brk(0x564218e10000)                     = 0x564018e28000
mmap(NULL, 8590069760, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = -1 ENOMEM (Cannot allocate memory)
write(2, "memory allocation of ", 21memory allocation of )   = 21
write(2, "8589934592", 108589934592)              = 10
write(2, " bytes failed", 13 bytes failed)           = 13
rt_sigprocmask(SIG_UNBLOCK, [ABRT], NULL, 8) = 0
rt_sigprocmask(SIG_BLOCK, ~[RTMIN RT_1], [], 8) = 0
getpid()                                = 5867
gettid()                                = 5867
tgkill(5867, 5867, SIGABRT)             = 0
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
--- SIGABRT {si_signo=SIGABRT, si_code=SI_TKILL, si_pid=5867, si_uid=1000} ---
+++ killed by SIGABRT (core dumped) +++
```

Notice that we get an `ENOMEM`. Rust's standard library emits a nice error message and then exits.

Now here's the strace for jemalloc:

```
mmap(0x7f42b4e00000, 4294967296, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_NORESERVE, -1, 0) = 0x7f40b4e00000
munmap(0x7f40b4e00000, 4294967296)      = 0
mmap(NULL, 8589934592, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_NORESERVE, -1, 0) = 0x7f3fb4e00000
+++ killed by SIGKILL +++
```

This is where get OOM killed. In order to make sense of what's happening here, we actually need to inspect our system's [`overcommit_memory`](https://www.kernel.org/doc/Documentation/sysctl/vm.txt) setting (`CTRL-F` for `overcommit_memory:`). In particular, from `man 5 proc`, it says:

> In  mode  0,  calls of mmap(2) with MAP_NORESERVE are not checked, and the default check is very weak, leading to the risk of getting a process "OOM-killed".

In my case, my system is using the default, which is indeed `0`. We can see that jemalloc is actually using `MAP_NORESERVE`, which means it falls into the realm of the OOM killer. The system allocator does not use `MAP_NORESERVE`, and based on `man 5 proc`, this seems to mean that calls to `malloc` can actually indeed fail. Which gives us the nice error message.

So, does this mean switching to the system allocator gives us a nice error message? The answer here is, "probably, if overcommit is set to the default, but otherwise, no, it does not." For example, if we set `overcommit = 1` (via `sudo sh -c 'echo 1 > /proc/sys/vm/overcommit_memory'`), then we can actually cause the system allocator to give us OOM:

```
mremap(0x7f80f4c4e000, 4294971392, 8589938688, MREMAP_MAYMOVE) = 0x7f7ef4c4d000
+++ killed by SIGKILL +++
```

Which means we're back to bad error messages, even with the system allocator. Moreover, GNU grep exhibits exactly the same behavior as ripgrep with `overcommit = 1`. It exits with a cryptic `Killed` message.

Interestingly, it is possible to get a good error message even with jemalloc, and this can be done by setting `overcommit = 2`, which effectively disables the illusion that the system have gobs of memory to lend out, even when it doesn't. Here's the strace for this case:

```
mmap(0x7faabda00000, 2147483648, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = -1 ENOMEM (Cannot allocate memory)
mmap(NULL, 4294967296, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = -1 ENOMEM (Cannot allocate memory)
brk(0x562899a00000)                     = 0x562799861000
write(2, "memory allocation of ", 21memory allocation of )   = 21
write(2, "4294967296", 104294967296)              = 10
write(2, " bytes failed", 13 bytes failed)           = 13
rt_sigprocmask(SIG_UNBLOCK, [ABRT], NULL, 8) = 0
rt_sigprocmask(SIG_BLOCK, ~[RTMIN RT_1], [], 8) = 0
getpid()                                = 6747
gettid()                                = 6747
tgkill(6747, 6747, SIGABRT)             = 0
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
--- SIGABRT {si_signo=SIGABRT, si_code=SI_TKILL, si_pid=6747, si_uid=1000} ---
+++ killed by SIGABRT (core dumped) +++
```

Interestingly, jemalloc actually seems to react to the change in the `overcommit_memory` setting, and no longer uses `MAP_NORESERVE`, presumably because there is no way to make use of overcommit since it has been disabled.

So... I don't think there is much to be done for the OOM condition specifically. If you want better out-of-memory errors, that it seems like you just need to disable overcommit.

---

_Comment by @Ekleog on 2018-07-27 02:47_

#### Meta-talk

> Apologies for the aggression, my frustration got the better of me. Your comments have not be easy to read or respond to. As such, I will try to be brief and to the point.

Sorry about that! I re-read my first post and indeed I at the same time left parts of the issue template unfilled and forgot to mention the `-a` flag… that must not have been easy to read indeed. I will try to plead guilty to having posted the issue in a rush after coming back from work (where I encountered the issue without access to GitHub), but that's clearly my mistake and i should have waited to have enough time to first sleep and then file a proper bug report written in a non-telegraphic way. :)

> I will not be spending my time improving the failure mode of OOM conditions. I agree that GNU grep's failure mode is better. I will not be beholden to nitpicks about particular wordings of project goals. (The word "goal" doesn't appear once in the project README, largely to avoid that style of argumentation, which I find incredibly frustrating as a maintainer.) I would be happy if someone else wanted to improve this failure mode, so long as it didn't impose any significant complications on the code. I don't see any reason to track this particular issue.

Sorry also if I appeared to be arguing on the wording of the README too, I only wanted to point out that *if* the goal of ripgrep was also to have fewer failing conditions, *then* such a change would be a step in the right direction -- to the cost of much more complex code indeed. Most “improvements” are trade-offs between code complexity and the usefulness of the improvement, and I can understand that this one doesn't pass your threshold, I was just surprised to read that an OOM was “working as intended” :)

#### Long-term goal

> My long term goal is to attempt to address this in the regex engine, if it's possible. If that effort succeeds, then it may bear fruit that ripgrep can benefit from. I don't see any reason to track this particular issue, as the next immediate work here comes in the regex engine, which is already being tracked.

Oh I didn't know that! this sounds great indeed :)

#### Medium-term work-around

(short-term work-around being setting the system to no overcommit)

> OK, I decided to do an experiment and actually look at what's going on in terms of allocation, since I wanted to know whether moving to the system allocator would actually be a plausible solution to this problem. The short answer is, no, it wouldn't be.

Hmm… my reading of your post sounds like it would be a solution for the default case, which should be the most frequent one? [Documentation about overcommit](https://www.kernel.org/doc/Documentation/vm/overcommit-accounting.rst) seems (to me) to imply that it should not be set to 1 by default in any non-specialist distribution, and people manually changing their overcommit settings should expect the effects of these settings (ie. the kernel lying to applications requiring memory) to potentially change the behaviour of applications.

> So... I don't think there is much to be done for the OOM condition specifically. If you want better out-of-memory errors, that it seems like you just need to disable overcommit.

Apart from using the system allocator (great finding!), I can think of two potential other options:
1. Refuse allocating more than X RAM (or X% of the RAM?) at a time for the line buffer (if I understood correctly where the memory usage comes from) without an explicit flag
2. Use `MAP_POPULATE`, which seems to [force actual non-overcommit'd allocation](https://stackoverflow.com/questions/28267364/malloc-in-linux-there-is-no-guarantee-that-the-memory-really-is-available/28267661#28267661)

In both cases, I must say I have no idea of how hard it would be to integrate in jemalloc or the system allocator (even though idea 1 could likely be done with a wrapper around `Vec::new` or the equivalent function ripgrep currently uses for buffer allocation)

#### `-j1` weirdness

Now, just to come back to the `-j1` weirdness, I can now confirm it's only a buq of `psrecord`: I get the following with `-j1` (lines truncated)
```
99.5  0.2  0:53.07 python3.4m psrecord rg -j1 -a qwertyuiop /dev/nvme0n1p3 --plot rg
43.1 53.2  0:25.78 rg -j1 -a qwertyuiop /dev/nvme0n1p3
```
And the following without it
```
101.  0.2  0:16.86 python3.4m psrecord rg -a qwertyuiop /dev/nvme0n1p3 --plot rg-j1.
47.0  0.5  0:07.83 rg -a qwertyuiop /dev/nvme0n1p3
45.0  0.5  0:07.37 rg -a qwertyuiop /dev/nvme0n1p3
 0.7  0.5  0:00.06 rg -a qwertyuiop /dev/nvme0n1p3
 0.7  0.5  0:00.06 rg -a qwertyuiop /dev/nvme0n1p3
 0.7  0.5  0:00.06 rg -a qwertyuiop /dev/nvme0n1p3
 0.7  0.5  0:00.06 rg -a qwertyuiop /dev/nvme0n1p3
 0.7  0.5  0:00.06 rg -a qwertyuiop /dev/nvme0n1p3
 0.0  0.5  0:00.05 rg -a qwertyuiop /dev/nvme0n1p3
 0.0  0.5  0:00.05 rg -a qwertyuiop /dev/nvme0n1p3
```

#### `-z` / `--null-data`

> The -z/--null-data flag sounds like a good addition to ripgrep. This issue can track that feature.

Sounds great, thanks!

---

_Comment by @BurntSushi on 2018-07-27 15:00_

Thanks for the follow up. I don't have the energy to debate the various details or your proposed solutions (none of which I'd be comfortable with), but I think ripgrep's current behavior on OOM is consistent with your `overcommit_memory = 0` setting. If you want better failure messages on OOM, then disable overcommit. I don't know why you brought up `overcommit_memory = 1` since that totally prevents a good failure modes regardless, even when using GNU grep. To disable overcommit, you need to set `overcommit_memory = 2`.

---

_Comment by @Ekleog on 2018-07-27 18:23_

… wait, *you* brought up `overcommit_memory = 1`, and from my understanding of your post it was the reason why you think it isn't possible to handle OOM better:

> So, does this mean switching to the system allocator gives us a nice error message? The answer here is, "probably, if overcommit is set to the default, but otherwise, no, it does not." For example, if we set overcommit = 1 (via sudo sh -c 'echo 1 > /proc/sys/vm/overcommit_memory'), then we can actually cause the system allocator to give us OOM: […]

Did I completely misunderstand your post on this matter?

-----

Anyway, this changes nothing to the fact that you feel uncomfortable with the two proposed solutions, and this ends the debate, so let's have this issue track only `-z` / `--null-data` and not potential OOM-handling improvements :)

---

_Comment by @BurntSushi on 2018-07-27 19:24_

`overcommit_memory = 1` enables overcommit. `overcommit_memory = 0` _heuristically_ enables overcommit in a way that will vary from allocator to allocator (the system allocator does not seem to take advantage of it, but jemalloc does). `overcommit_memory = 2` completely disables overcommit. If you want better OOM error messages, then completely disable overcommit via `overcommit_memory = 2`. My comment above demonstrates that, with `overcommit_memory = 1`, **both** the system allocator and jemalloc get OOM killed and present bad failure modes. Given that you've specifically expressed distaste with the `Killed` error message, this is decidedly **not** what you want. At no point did I ever suggest that you or anyone else should set `overcommit_memory = 1`.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

_Comment by @sergeevabc on 2020-03-07 00:20_

@BurntSushi, thank you for adding ```--null-data```.

---

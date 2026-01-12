```yaml
number: 1550
title: high peak memory usage with parallel breadth first directory traversal
type: issue
state: closed
author: Shnatsel
labels:
  - bug
assignees: []
created_at: 2020-04-12T21:50:32Z
updated_at: 2020-04-18T15:33:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1550
synced_at: 2026-01-12T16:13:23Z
```

# high peak memory usage with parallel breadth first directory traversal

---

_@Shnatsel_

#### What version of ripgrep are you using?

ripgrep 12.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`cargo install ripgrep` on `rustc 1.42.0 (b8cedc004 2020-03-09)`

#### What operating system are you using ripgrep on?

Linux, derived from Ubuntu 18.04

#### Describe your question, feature request, or bug.

rg seems to leak memory when searching for an expression in latest versions of all crates from crates.io. The memory usage gradually increases and peaks at around 1Gb.

#### If this is a bug, what are the steps to reproduce the behavior?

```bash
# get all crates from crates.io
git clone https://github.com/the-lean-crate/criner && cd criner && cargo run --release -- mine
# get latest version of every crate
for dir in ./criner.db/assets/*/*/*/; do echo -n "$dir"; ls "$dir" | grep '\.crate' | sort -Vr | head -n 1; done > ./latest-versions
mkdir crates
cd crates
# decompress the latest version of every crate
cat ../latest-versions | parallel ../decompress-one-crate.sh
```
where decompress-one-crate.sh is
```bash
#!/bin/sh
cat "$1" | tar --gunzip -xf -
```

Sample `rg` invocation exhibiting the behavior is:

```
rg --iglob '*.rs' -iF '#[allow('
```

#### If this is a bug, what is the actual behavior?

Memory usage increases as the search progresses, reaching as high as 1Gb. It increases gradually and accumulates.

#### If this is a bug, what is the expected behavior?

Memory usage should not accumulate


---

_Comment by @Shnatsel on 2020-04-12 21:55_

Using a regex instead of a simple pattern doesn't seem to make a difference - this one behaves identically: `rg --iglob '*.rs' -U "enum[^\\n]+\\{\\n[^\\n]+\\n\\}"`

---

_Comment by @Shnatsel on 2020-04-12 22:02_

Downloading all of crates.io takes 50Gb of disk space, and latest version of every crate uncompressed is 18Gb. If you don't want to download that, point me to a memory profiler and I can run it myself now that I have the corpus locally.

---

_Comment by @BurntSushi on 2020-04-12 22:10_

I've actually been meaning to download all crates, since it makes for a nice test corpus. So I will investigate this. Thank you for making it easy.

Without investigating, I'm pretty sure I know the culprit. If I'm right, then it's less of a leak and more of a conscious trade off. Namely, ripgrep must buffer enough room to store at least one line contiguously in memory. If you run across a very long line, then the buffer expands to that capacity. But it will never shrink itself. ripgrep uses one of these buffers per thread. So if each buffer has to expand to a large size, then they never contract. While contracting won't reduce per-thread peak memory usage, it could reduce the overall process peak memory usage in most cases.

With that said, there is no particular reason why the behavior must be this way. It's just that I've never had a good test case for it. Some kind of contraction is probably wise. Although whether contraction actually results in memory being freed back to the OS is perhaps a separate issue that I suspect is mostly out of the control of ripgrep.

But yeah, I'll do some investigation to confirm my hypothesis. Thanks for the report!

---

_Label `bug` added by @BurntSushi on 2020-04-12 22:10_

---

_Label `question` added by @BurntSushi on 2020-04-12 22:10_

---

_Label `question` removed by @BurntSushi on 2020-04-18 00:30_

---

_Renamed from "Memory leak when searching all of crates.io" to "high peak memory usage with parallel breadth first directory traversal" by @BurntSushi on 2020-04-18 00:31_

---

_Comment by @BurntSushi on 2020-04-18 00:38_

OK, I did some experimenting and my guess was wrong.

It turns out that this is a result of how ripgrep implements parallel directory traversal. Namely, since it uses a queue to communicate new work among threads, this results in a breadth first traversal. In this case, the initial directory is extremely wide, because it contains the latest version of every crate. Upon entering each directory, a matcher for processing gitignore rules is created. Since this is breadth first, a gitignore matcher is created for _every directory_ before ripgrep ever starts to process a single file. This is primarily what contributes to a large peak memory usage. You can test this hypothesis by searching without parallelism (with `-j1`) or by disabling gitignore exclusions (with `-u/--no-ignore`). Both result in very small peak memory usage.

Fixing this is unfortunately not quite so easy. @bmalehorn was actually barking up the right tree a few years ago, and even submitted a PR to switch the traversal to depth first in #450. But the upsides didn't seem so apparent to me then, and it appeared to change the output order a bit too much for my taste. But with this example in front of me, it seems clearer that switching to a depth first traversal is better than what we have today.

Since then though, Crossbeam seems to no longer provide a thread safe stack. One can be [implemented without much code](https://github.com/crossbeam-rs/crossbeam/blob/e3020305ffa5e32917c0122e24578a486e34cfbd/crossbeam-epoch/examples/treiber_stack.rs), but I'm hesitant to add `unsafe`, especially when it's in an area of data structures that are outside my area of expertise.

So I think this bug is blocked on either rethinking how traversal works (I've always wanted to use rayon) or switching to using a depth first traversal with minimal risk from `unsafe`.

---

_Comment by @Shnatsel on 2020-04-18 01:42_

I've consulted my friendly neighbourhood good of concurrency, Acrimon, and their recommendation for a concurrent stack was `Mutex<Vec>` because a Trieber stack is not meaningfully faster than that. Faster concurrent stack implementations are possible, but "no one is really insane enough to do this stuff".

Listing all files up front before creating the gitignore handlers would solve this, but also create a different kind of scalability limit with all the file paths being loaded in memory at once, and I'm not sure how that interacts with filesystem cache behavior.

---

_Comment by @BurntSushi on 2020-04-18 01:54_

A `Mutex<Vec<Work>>` is indeed worth trying and shouldn't be too hard to do so. It is plausible that we won't be able to observe a performance difference.

Listing all files up front is another way of looking at, but is a major architectural change and it is difficult for me to reason about its performance characteristics.

---

_Closed by @BurntSushi on 2020-04-18 15:33_

---

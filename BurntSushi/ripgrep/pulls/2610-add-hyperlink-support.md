```yaml
number: 2610
title: add hyperlink support
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/hyperlinks
created_at: 2023-09-21T17:18:23Z
updated_at: 2023-09-26T21:13:54Z
url: https://github.com/BurntSushi/ripgrep/pull/2610
synced_at: 2026-01-12T18:23:14Z
```

# add hyperlink support

---

_@BurntSushi_

This is based on #2483 and keeps its general structure, but rejiggers some details. The biggest change is probably lifting the `gethostname` call from the `grep-printer` crate all the way up into ripgrep's core. We also add a `--hostname-bin` flag that allows the end user to provide an executable program that prints the hostname ripgrep should use.

Closes #665 

---

_Comment by @ltrzesniewski on 2023-09-22 21:21_

I know this is a WIP but I took a quick glance and thought I'd mention it just in case: you removed the `OnceCell` which I put there so that only matching files would get their paths canonicalized.


---

_Comment by @BurntSushi on 2023-09-22 21:49_

Oh, hmmm. The point about a non-match is a good one. I'll take a closer look. Thank you for catching that!

---

_Comment by @BurntSushi on 2023-09-23 12:08_

Yes, I see, that's a good catch. Arguably other stuff in a `PrinterPath` should be behind `OnceCell` too. Good call.

---

_Merged by @BurntSushi on 2023-09-25 18:39_

---

_Closed by @BurntSushi on 2023-09-25 18:39_

---

_Branch deleted on 2023-09-25 18:39_

---

_Comment by @ltrzesniewski on 2023-09-26 20:49_

May I bother you with a little question I was wondering about?

I initially used `write!` to format numbers into a buffer, (for instance: `Part::Line => write!(output, "{}", values.line)`). From what I could gather, this is the standard way of doing it, and it apparently doesn't allocate.

You replaced those with explicit `to_string()` calls, such as:

```rust
let line = values.line.unwrap_or(1).to_string();
dest.extend_from_slice(line.as_bytes());
```

I was wondering why? Is the optimizer able to remove the heap allocation, or does it simply not matter in this context and you prefer that code style?


---

_Comment by @BurntSushi on 2023-09-26 20:59_

Great question! And feel free to ask more like it.

I replaced them because I have a faint recollection that `write!` goes through the formatting machinery. I can't remember whether it allocates or not, but my recollection is that the formatting machinery had more cost than just doing `n.to_string()`. I note, for example, that `n.to_string()` is how line numbers (and column numbers and byte offsets) have been written for a while. So I deferred to past experience and copied that technique.

It's funny you bring it up, because I actually have a TODO (before the next release) to try and more carefully benchmark this choice. I wanted to bake off four different choices:

1. Status quo.
2. `write!("{}", n)`.
3. The [`itoa`](https://docs.rs/itoa/latest/itoa/) crate.
4. A simple hand-rolled version that doesn't allocate or use `unsafe`.

In particular, I want to run benchmarks both with the system allocator (glibc on my system) and with musl's allocator, which has been a bit slower in my experience.

---

_Comment by @ltrzesniewski on 2023-09-26 21:13_

Thanks! 🙂 

---

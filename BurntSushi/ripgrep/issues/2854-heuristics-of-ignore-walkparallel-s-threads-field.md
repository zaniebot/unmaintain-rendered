```yaml
number: 2854
title: "Heuristics of `ignore::WalkParallel`'s `threads` field: hardcoded to 2?"
type: issue
state: closed
author: alexpovel
labels:
  - rollup
assignees: []
created_at: 2024-07-16T18:24:04Z
updated_at: 2025-09-20T01:08:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2854
synced_at: 2026-01-12T16:13:25Z
```

# Heuristics of `ignore::WalkParallel`'s `threads` field: hardcoded to 2?

---

_@alexpovel_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ignore = "0.4.22"

### How did you install ripgrep?

n/a

### What operating system are you using ripgrep on?

n/a

### Describe your bug.

I'm using `WalkParallel` for, well, parallel, recursive directory traversal. There's a `threads` parameter which I left unspecified (using `WalkBuilder`):

https://github.com/BurntSushi/ripgrep/blob/71d71d2d98964653cdfcfa315802f518664759d7/crates/ignore/src/walk.rs#L1196

The docs say if left unspecified, aka at `0`, the number of threads will be picked based on heuristics:

https://github.com/BurntSushi/ripgrep/blob/71d71d2d98964653cdfcfa315802f518664759d7/crates/ignore/src/walk.rs#L643-L644



### What are the steps to reproduce the behavior?

Execute

```rust
ignore::WalkBuilder::new(&some_path)
        .build_parallel()
        .run(todo!())
```

### What is the actual behavior?

Using these settings, I noticed my application using 200% CPU, where more cores would have been available. 

### What is the expected behavior?

Processing to use more threads, as determined by the heuristics.

It turns out, `threads` is hard-coded to 2 if unspecified:

https://github.com/BurntSushi/ripgrep/blob/71d71d2d98964653cdfcfa315802f518664759d7/crates/ignore/src/walk.rs#L1307-L1308

where for the 'heuristic', I expected something along the lines of `num_cpus`. I can confirm setting `threads` to [`num_cpus::get()`](https://docs.rs/num_cpus/latest/num_cpus/fn.get.html), 12 in my case, yields results as expected.

Could/should this be used as the heuristic? It is a substantial, free win in my case.

If not, then the docs should probably be adjusted.

---

_Comment by @BurntSushi on 2024-07-16 18:34_

It is still technically a heuristic, although not a very good one. :-)

I think I'd be okay if you copied ripgrep's heuristic to `ignore`:

https://github.com/BurntSushi/ripgrep/blob/71d71d2d98964653cdfcfa315802f518664759d7/crates/core/flags/hiargs.rs#L172

And use `std`, not an extra crate.

---

_Comment by @alexpovel on 2024-07-16 18:51_

Thanks for the quick reply and pointer to ripgrep's implementation! I'll get onto that.

---

_Label `rollup` added by @BurntSushi on 2025-07-26 14:25_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

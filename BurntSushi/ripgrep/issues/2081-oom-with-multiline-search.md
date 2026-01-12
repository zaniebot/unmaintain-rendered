```yaml
number: 2081
title: OOM with multiline search 
type: issue
state: closed
author: the8472
labels:
  - wontfix
assignees: []
created_at: 2021-11-22T18:34:24Z
updated_at: 2023-11-24T20:08:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2081
synced_at: 2026-01-12T16:13:24Z
```

# OOM with multiline search 

---

_@the8472_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

arch linux, 5.14.15-arch1-1

#### Describe your bug.

When running `rg -C3 --multiline --multiline-dotall "does not exist.{0,200}does not exit"` on a directory tree containing 70GB of `perf` recordings ripgrep freezes the system until it gets OOM-killed.

#### What are the steps to reproduce the behavior?

Locally I reproduce it by

1. cloning the rust-lang repo
2. `perf record --call-graph dwarf -e instructions,cycles ./x.py test --stage 1 src/test/ui` - this will take a while depending on your hardware and take up about 80GB of disk space
3. run `rg -C3 --multiline --multiline-dotall "does not exist.{0,200}does not exit"` 

It most likely doesn't have to be the rust repo itself, just any profiling session that generates huge `perf.data`.

The issue does not reproduce without the `--multiline-dotall`, neither does it if I delete `perf.data`.

#### What is the actual behavior?

https://gist.githubusercontent.com/the8472/d43602305da791704e5c318e7f1d4163/raw/8efbbf267064f362c25238572586f89ba5cafed7/gistfile1.txt

#### What is the expected behavior?

Not OOM or maybe print a warning before attempting to proceed anyway.

---

_Comment by @BurntSushi on 2021-11-23 16:07_

It will be a bit before I'm able to invest time in this, but it is worth pointing out that "ripgrep OOMs" is not in and of itself unexpected or a bug unfortunately. It's non-ideal, for sure, but it's difficult to do otherwise. GNU grep has the same problem.

Ignoring multi-line mode for a moment, ripgrep can use unbounded memory usage because it doesn't place a limit on how long a line can be. If a single line is GBs, ripgrep won't stop reading until it is either forced to stop or until it sees a `\n`. There aren't too many other choices here.

* In theory, ripgrep could use fallible allocation APIs, and if an allocation fails, somehow abort the process. But this is little different than the status quo, and would only work if overcommit isn't enabled anyway.
* Another possibility is to permit the user to manually set a memory limit and ripgrep would try not to allocate more than for its line buffer. But ripgrep does a lot more allocation than just its line buffer, and not all of which is easily controlled. (For example, allocation at various points while doing directory tree traversal.)
* Like the former, but with ripgrep trying to automatically detect the memory limit. In addition to the problems with the former, you also now have the problem of devising a heuristic to determine what the memory limit should be.

ripgrep has other problems with memory, some of which are specifically related to your use case:

* In multi-line mode, ripgrep has no practical choice but to either use memory maps or read the entire file into memory. Rust's regex engine doesn't support searching non-contiguous strings, so you're kind of hosed here.
* When using parallelism, ripgrep has to buffer all of the search results for each file. So if your search produces lots of results, that can influence memory usage as well. My guess is that your query is designed not to matching anything though, so it seems unlikely that this is a problem.
* Any regex involving big bounded repeats (like `.{0,200}`) tends to be quite memory hungry. Now, `.` itself is pretty small, so this isn't particularly egregious. (Using `\w{0,200}` for example would be around 60 times bigger, to a first approximation.)

Now the interesting bit here is that `(?s:.)` provokes the problem but `.` doesn't. That is pretty interesting. If you're willing to do a little debugging, you might try:

* A search with `--mmap` and another with `--no-mmap`.
* A search with `-j1` to force single threaded mode.
* A search with `-P` to force use of PCRE2 to see how it fairs here.

---

_Comment by @BurntSushi on 2023-11-24 20:08_

I think my last comment on this issue pretty much covers what I know here. It's unfortunate but OOM is difficult to avoid given ripgrep's design and goals.

---

_Closed by @BurntSushi on 2023-11-24 20:08_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:08_

---

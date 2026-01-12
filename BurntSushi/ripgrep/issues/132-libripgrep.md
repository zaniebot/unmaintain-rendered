```yaml
number: 132
title: libripgrep?
type: issue
state: closed
author: stephanbuys
labels: []
assignees: []
created_at: 2016-09-29T07:18:31Z
updated_at: 2019-05-21T02:31:17Z
url: https://github.com/BurntSushi/ripgrep/issues/132
synced_at: 2026-01-12T16:13:21Z
```

# libripgrep?

---

_@stephanbuys_

After reading your post and seeing the truly amazing depths that you've gone to with the regex library and other methods that you use to make ripgrep blazingly fast I started wondering about the "other methods" and if there's a use-case for something like `librip` that can be used for fast searching over arbitrary streams/blobs of data?


---

_Comment by @BurntSushi on 2016-09-29 11:22_

Have you looked at the source code? What about the [`Cargo.toml`](https://github.com/BurntSushi/ripgrep/blob/master/Cargo.toml)? `ripgrep` itself is relatively small. Most of the performance it gets comes from the regex engine, [which is already a separate library](https://github.com/rust-lang-nursery/regex). There are a small number of regex-oriented optimizations that `ripgrep` performs that are specific to line-based searching, but even these are already factored out into [a separate `grep` library](http://burntsushi.net/rustdoc/grep/).

With that said, there's plenty of stuff I'd like to move out of `ripgrep` into standalone libraries:
- The search code that handles things like counting lines, contexts, inverted matches, etc. This is in both `src/search_stream.rs` and `src/search_buffer.rs`. I started by writing this inside of `grep`, but couldn't make an iterator style API work. That's fine, but that means it needs more design work.
- The code to process ignore patterns is essentially the rest of `ripgrep`. `ripgrep` has its own glob implementation built on regexes in `src/glob.rs`, it implements gitignore semantics in `src/gitignore.rs`, it does file type matching in `src/types.rs` and finally ties everything together in `src/ignore.rs`.
- There is a comparatively small amount of code that does directory traversal and the actual task of shoveling work off to search workers.

I'd like to at least move the first two things in separate libraries. Doing it for globs/ignores is probably my highest priority, because it needs refactoring and I'd like to move it toward a design that can benefit from parallelism.

In any case, I don't think we need a ticket tracking this. It will happen over time as the code improves.


---

_Closed by @BurntSushi on 2016-09-29 11:22_

---

_Comment by @stephanbuys on 2016-09-29 11:25_

@BurntSushi - sounds awesome, will monitor the progress with interest. Keep up the great work!


---

_Comment by @mrageh on 2019-05-20 20:32_

Firstly thank you for building and maintaining Ripgrep, which is a fantastic library.

I'm new to Rust and Ripgrep seems like it's grown considerably since this issue was closed. I'd like to use it as a library in a toy project I'm working on. Specifically what I'd like to do is use it to search for tokens/terms. At the moment in order to use Ripgrep I have to shell out to the terminal. 

What would you recommend I do, use https://github.com/rust-lang/regex?

---

_Comment by @mrageh on 2019-05-21 02:31_

Please ignore me as I just found https://github.com/BurntSushi/ripgrep/issues/1009

---

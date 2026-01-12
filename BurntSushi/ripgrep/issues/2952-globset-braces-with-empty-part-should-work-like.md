```yaml
number: 2952
title: globset braces with empty part should work like in bash
type: issue
state: closed
author: nalply
labels: []
assignees: []
created_at: 2024-12-22T15:49:41Z
updated_at: 2024-12-22T15:54:52Z
url: https://github.com/BurntSushi/ripgrep/issues/2952
synced_at: 2026-01-12T16:13:25Z
```

# globset braces with empty part should work like in bash

---

_@nalply_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

It's an issue with globset@0.4

### How did you install ripgrep?

`cargo add globset@0.4` for my project, but do see the gist for the minimal reproducing example

### What operating system are you using ripgrep on?

Arch Linux

### Describe your bug.

The glob `"test{,2}"` should match `"test"`, but it does not.

Compare this bash behavior:

```
> touch test test2
> echo test{,2}
```

produces `test test2`.

### What are the steps to reproduce the behavior?

Compile and run this file: https://gist.github.com/nalply/95992067f1e1c94877a2d2af536ef66c

Alternatively use `cargo eval globset_bug.rs`

### What is the actual behavior?

```
> cargo eval globset_bug.rs
   Compiling memchr v2.7.4
   Compiling regex-syntax v0.8.5
   Compiling log v0.4.22
   Compiling aho-corasick v1.1.3
   Compiling bstr v1.11.1
   Compiling regex-automata v0.4.9
   Compiling globset v0.4.15
   Compiling rplay v0.1.0 (/home/nalp/.cache/cargo-eval/scripts/file-rplay-e291303ba1ab48b6)
    Finished `release` profile [optimized] target(s) in 5.46s
false
```

### What is the expected behavior?

```
> cargo eval globset_bug.rs
   Compiling memchr v2.7.4
   Compiling regex-syntax v0.8.5
   Compiling log v0.4.22
   Compiling aho-corasick v1.1.3
   Compiling bstr v1.11.1
   Compiling regex-automata v0.4.9
   Compiling globset v0.4.15
   Compiling rplay v0.1.0 (/home/nalp/.cache/cargo-eval/scripts/file-rplay-e291303ba1ab48b6)
    Finished `release` profile [optimized] target(s) in 5.46s
true
```

---

_Comment by @BurntSushi on 2024-12-22 15:52_

Have you tried https://docs.rs/globset/latest/globset/struct.GlobBuilder.html#method.empty_alternates?

---

_Comment by @nalply on 2024-12-22 15:54_

no, sorry, my bad, closing immediately.

---

_Closed by @nalply on 2024-12-22 15:54_

---

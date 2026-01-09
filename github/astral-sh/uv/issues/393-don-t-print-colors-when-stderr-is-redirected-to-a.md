---
number: 393
title: "Don't print colors when stderr is redirected to a file"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-10T16:47:05Z
updated_at: 2023-11-13T20:00:14Z
url: https://github.com/astral-sh/uv/issues/393
synced_at: 2026-01-07T13:12:16-06:00
---

# Don't print colors when stderr is redirected to a file

---

_Issue opened by @konstin on 2023-11-10 16:47_

When using

```
cargo run --bin puffin-dev -q -- resolve-cli "transformers[tensorboard]" 2> output.txt
```

`output.txt` contains color sequences. Colors should only be shown when the stream they are shown on is a tty.

---

_Comment by @charliermarsh on 2023-11-10 16:58_

Does this happen with `pip-compile`? I thought I fixed it.

---

_Comment by @konstin on 2023-11-10 17:02_

No there it works! Do you know where that setting is set?

---

_Label `bug` added by @charliermarsh on 2023-11-10 17:10_

---

_Comment by @charliermarsh on 2023-11-10 17:10_

In `pip_compile.rs`, we add:

```rust
    if output_file.is_some() {
        colored::control::set_override(false);
    }
```

---

_Comment by @charliermarsh on 2023-11-10 17:11_

I don't know if there's a better solution.

---

_Comment by @charliermarsh on 2023-11-10 18:27_

I think https://docs.rs/anstream/latest/anstream/ maybe helps with this problem? @BurntSushi might have ideas too.

---

_Comment by @BurntSushi on 2023-11-10 18:58_

anstream is somewhat orthogonal to whether colors are enabled or not I think. (Although it may solve that problem.) To detect whether `stderr` is a tty or not, you want this:

```rust
use std::io::IsTerminal;

if std::io::stderr().is_terminal() {
    println!("stderr is a tty!");
} else {
    println!("stderr is NOT a tty");
}
```

The `std::io::IsTerminal` trait is a somewhat recent addition to the standard library. It was added in Rust 1.70.

---

_Comment by @charliermarsh on 2023-11-10 20:29_

Hmm, why does Ruff not suffer from this problem? We don't print colors when you pipe stdout to a file.

---

_Comment by @charliermarsh on 2023-11-10 20:57_

Maybe `colorize` handles writing stdout but not stderr or something, I don't know.

---

_Comment by @konstin on 2023-11-13 11:47_

Yep it's stdout vs. stderr:
```
cargo run --bin puffin-dev -q -- resolve-cli "transformers[tensorboard]" 2> output.txt > /dev/null
```
writes the correct output.txt, but without `> /dev/null` i see the color codes.

---

_Comment by @BurntSushi on 2023-11-13 12:32_

> I think https://docs.rs/anstream/latest/anstream/ maybe helps with this problem? @BurntSushi might have ideas too.

After a quick look, I believe `anstream` gets this right. In particular, its [`AutoStream`](https://docs.rs/anstream/latest/anstream/struct.AutoStream.html) abstraction. In particular, it depends on [`RawStream`](https://docs.rs/anstream/latest/anstream/stream/trait.RawStream.html) which in turn depends the [`IsTerminal`](https://docs.rs/anstream/latest/anstream/stream/trait.IsTerminal.html) trait. (It looks like it's a copy of the standard library's trait, probably because it was published before `IsTerminal` was stable?) In any case, it doesn't look like it gets confused about stdout versus stderr.

Looking at [`colorize`](https://docs.rs/colorize/latest/colorize/), it exists at a lower level of abstraction. I don't believe it concerns itself with what its actually writing to or whether colors _ought_ to be enabled.

Oh, Puffin is using [`colored`](https://docs.rs/colored/latest/colored/), not `colorize`. And assuming Puffin is using its `ShouldColorize` abstraction, it does indeed appear to be [hard-coded to assuming all output goes to stdout](https://docs.rs/colored/latest/src/colored/control.rs.html#106).

This looks like it might be a little tricky to fix without some refactoring. For example, this code prints to stderr directly using colorize's functions:

https://github.com/astral-sh/puffin/blob/81c9cd0d4ad44920e3785fcb4a499e4f559c77f2/crates/puffin-cli/src/main.rs#L315-L331

Same deal with `puffin-dev`:

https://github.com/astral-sh/puffin/blob/81c9cd0d4ad44920e3785fcb4a499e4f559c77f2/crates/puffin-dev/src/main.rs#L96-L105

Oh, maybe these are the only places? But in any case, I think they can't be correct because you can't really use `IsTerminal` in a global way like `colorize` is doing. You need separate detection logic based on where you're writing.

So long story short, maybe there are two choices here:

1. If there's only a couple places where we print colors to stderr, then do our own `std::io::stderr().is_terminal()` check and only use colors if it's a terminal. Probably an easy fix.
2. Refactor to use a more principled solution like `anstream`.

---

_Comment by @charliermarsh on 2023-11-13 14:41_

Thanks @BurntSushi -- this matches my understanding too. `anstream` abstracts away an underlying writer and inserts or removes colors based on the capabilities. It looks like they do have an adapter for `owo_colors` which we could consider using instead.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-13 19:22_

---

_Comment by @charliermarsh on 2023-11-13 19:22_

Gonna investigate this real quick.

---

_Referenced in [astral-sh/uv#415](../../astral-sh/uv/pulls/415.md) on 2023-11-13 19:50_

---

_Closed by @charliermarsh on 2023-11-13 20:00_

---

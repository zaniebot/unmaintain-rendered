```yaml
number: 5922
title: "`#[arg(long)]` should support multiple arguments by default when specified for `Vec` types"
type: issue
state: closed
author: zhiqiangxu
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2025-02-21T02:36:04Z
updated_at: 2025-02-21T14:36:01Z
url: https://github.com/clap-rs/clap/issues/5922
synced_at: 2026-01-12T16:14:17Z
```

# `#[arg(long)]` should support multiple arguments by default when specified for `Vec` types

---

_@zhiqiangxu_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Currently this won't make `peer_workers` accept multiple arguments:

```rust
    /// Peer workers
    #[arg(long)]
    peer_workers: Vec<String>,
```

If run with `--peer-workers w1 w2`, it'll report:

```bash
error: unexpected argument 'w2' found
```

If I specify it like this:

```rust
    /// Peer workers
    #[arg(long,num_args = 1..)]
    peer_workers: Vec<String>,
```

then it can accept multiple arguments.




### Describe the solution you'd like

`#[arg(long)]` should support multiple arguments by default when specified for `Vec` types, just like `#[arg(long,num_args = 1..)]`.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @zhiqiangxu on 2025-02-21 02:36_

---

_Label `A-derive` added by @epage on 2025-02-21 14:34_

---

_Comment by @epage on 2025-02-21 14:35_

These defaults for `Vec` are a very intentional choice.
- Multiple occurrences and value delimited options are much more common / idiomatic in CLIs than multiple values
- Looking over the warnings on [num_args](https://docs.rs/clap/latest/clap/struct.Arg.html#method.num_args), that is not something that people should automatically get but should explicitly opt-in to to be aware of the trade offs


---

_Closed by @epage on 2025-02-21 14:35_

---

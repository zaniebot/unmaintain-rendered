---
number: 5936
title: Use liberators instead of loop push to boost performance
type: issue
state: closed
author: Unparalleled-Calvin
labels:
  - C-enhancement
assignees: []
created_at: 2025-03-02T23:18:11Z
updated_at: 2025-03-04T14:24:42Z
url: https://github.com/clap-rs/clap/issues/5936
synced_at: 2026-01-07T13:12:20-06:00
---

# Use liberators instead of loop push to boost performance

---

_Issue opened by @Unparalleled-Calvin on 2025-03-02 23:18_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.31

### Describe your use case

This issue is related to running performance.
Usually loop push spends more time than using `Vec::extend` because the latter undergo partial optimization during implementation. For example, it will reserve memory according to the bound of the iterator.

### Describe the solution you'd like

An example is as follows.

clap_builder/src/mkeymap.rs line 179
```rust
for (short, _) in arg.short_aliases.iter() {
    let key = KeyType::Short(*short);
    keys.push(Key { key, index });
}
```

We suggest to change into this format:
```rust
keys.extend(arg.short_aliases.iter().map(|(short, _)| {
    let key = KeyType::Short(*short);
    Key { key, index }
}));
```

A draft PR is introduced in #5937 where other similar code is optimized.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Unparalleled-Calvin on 2025-03-02 23:18_

---

_Referenced in [clap-rs/clap#5937](../../clap-rs/clap/pulls/5937.md) on 2025-03-02 23:19_

---

_Comment by @epage on 2025-03-03 15:13_

Is this assumed by inspection or has there been an observed performance problem caused by this?

---

_Comment by @Unparalleled-Calvin on 2025-03-04 04:28_

Well, other benchmark result shows that using `extend` and `iterator` brings a higher performance than plain `push`. The reason is about the space reservation and unchecked memory access (safe) implemented by `Vec::extend` internally.

[A merged pull request](https://github.com/rust-lang/rust/pull/95981) in rust serves as an example of this optimization.

---

_Comment by @epage on 2025-03-04 14:24_

Without benchmarks for this use case, I'm going to go ahead and defer this for now.

---

_Closed by @epage on 2025-03-04 14:24_

---

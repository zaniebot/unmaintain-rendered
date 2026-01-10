---
number: 5955
title: "not full complete when input with ::"
type: issue
state: open
author: Eternity1987
labels:
  - C-bug
assignees: []
created_at: 2025-03-21T07:03:45Z
updated_at: 2025-03-21T11:14:51Z
url: https://github.com/clap-rs/clap/issues/5955
synced_at: 2026-01-10T01:28:19Z
---

# not full complete when input with ::

---

_Issue opened by @Eternity1987 on 2025-03-21 07:03_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.82.0 (f6e511eec 2024-10-15)

### Clap Version

4.5.32

### Minimal reproducible code

```rust
fn main() {}
```


### Steps to reproduce the bug with the above code

My command line has a positional parameter plugin_name. When I input this position, the tab key completion should show options like composition::Client, composition::Listener, composition::NodeLikeListener, composition::Server, and composition::Talker. Indeed, these completions appear, but it only completes up to composition::. When I type composition::T, it does not autocomplete to composition::Talker. Is this a bug? It seems that when :: is entered, the tab key completion does not work and does not trigger.  Would you like any help with this issue?

### Actual Behaviour

test@ubuntu-lts:~/workspace/example$ clap_demo component load ComponentManager composition 
composition::Talker    --spin-time            
composition::Listener  --help                 
test@ubuntu-lts:~/workspace/example$ clap_demo component load ComponentManager composition composition::

### Expected Behaviour

full compelte

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Eternity1987 on 2025-03-21 07:03_

---

_Comment by @epage on 2025-03-21 11:14_

Could you provide reproduction steps, ideally with minimal example code?

---

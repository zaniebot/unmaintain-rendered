---
number: 2746
title: Broken example clap_derive/examples/keyvalue.rs
type: issue
state: closed
author: svenstaro
labels:
  - C-bug
assignees: []
created_at: 2021-08-30T01:54:28Z
updated_at: 2021-08-30T04:00:02Z
url: https://github.com/clap-rs/clap/issues/2746
synced_at: 2026-01-10T01:27:25Z
---

# Broken example clap_derive/examples/keyvalue.rs

---

_Issue opened by @svenstaro on 2021-08-30 01:54_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0-nightly (5eacec9ec 2021-08-28)

### Clap Version

v3.0.0-beta.4

### Minimal reproducible code

The official example over here doesn't work as documented: https://github.com/clap-rs/clap/blob/master/clap_derive/examples/keyvalue.rs

### Steps to reproduce the bug with the above code

Broken:
```
git checkout v3.0.0-beta.4
cargo run --example keyvalue -- -D as=1 -D asdads=5
```

Working:
```
git checkout v3.0.0-beta.2
cargo run --example keyvalue -- -D as=1 -D asdads=5
```

### Actual Behaviour

Output:
```
error: The argument '-D <DEFINES>...' requires 1 values, but 2 were provided
```

### Expected Behaviour

Output:
```
Opt { defines: [("as", 1), ("asdads", 5)] }
```

### Additional Context

This must've broken somewhat recently.

### Debug Output

_No response_

---

_Label `T: bug` added by @svenstaro on 2021-08-30 01:54_

---

_Comment by @svenstaro on 2021-08-30 01:59_

I bisected it and it appears 3f94d17c71d8dea26133dbe107289c7f0c187499 is the first bad commit.

---

_Comment by @svenstaro on 2021-08-30 02:00_

The commit makes me think that this is perhaps not a bug but just an old example? I'm not knowledgeable enough in this project's development to be able to judge that. At any rate, _something_ is broken here, even if it's just the example. :)

---

_Comment by @svenstaro on 2021-08-30 02:05_

Ah, it would appear like the example is merely missing a `multiple_occurrences(true)`. I'll make a PR.

---

_Renamed from "number_of_values is broken in v3.0.0-beta.4, fine in v3.0.0-beta.2" to "Broken example clap_derive/examples/keyvalue.rs" by @svenstaro on 2021-08-30 02:05_

---

_Referenced in [clap-rs/clap#2747](../../clap-rs/clap/pulls/2747.md) on 2021-08-30 02:53_

---

_Closed by @pksunkara on 2021-08-30 04:00_

---

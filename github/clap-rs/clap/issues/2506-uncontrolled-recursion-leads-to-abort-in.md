---
number: 2506
title: Uncontrolled recursion leads to abort in deserialization
type: issue
state: closed
author: vireshk
labels:
  - C-bug
assignees: []
created_at: 2021-05-28T07:56:27Z
updated_at: 2021-05-28T09:13:23Z
url: https://github.com/clap-rs/clap/issues/2506
synced_at: 2026-01-07T13:12:19-06:00
---

# Uncontrolled recursion leads to abort in deserialization

---

_Issue opened by @vireshk on 2021-05-28 07:56_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.1 (9bc8c42bb 2021-05-09)

### Clap Version

v2.33.3

### Minimal reproducible code

```rust
fn main() {}
```


### Steps to reproduce the bug with the above code

Have this dependency in Carto.toml

clap = { version = "2.33.3",  features = ["yaml"] }

and do:

$ cargo audit -q

### Actual Behaviour

$ cargo audit -q
Crate:         yaml-rust
Version:       0.3.5
Title:         Uncontrolled recursion leads to abort in deserialization
Date:          2018-09-17
ID:            RUSTSEC-2018-0006
URL:           https://rustsec.org/advisories/RUSTSEC-2018-0006
Solution:      Upgrade to >=0.4.1
Dependency tree: 
yaml-rust 0.3.5
└── clap 2.33.3
    └── vhost-device-i2c 0.1.0

error: 1 vulnerability found!


### Expected Behaviour

No errors 

### Additional Context

https://buildkite.com/rust-vmm/vhost-device-ci/builds/13#c50950ee-b2cb-4726-896a-fe2ac287ad79

Link to the issue.

### Debug Output

_No response_

---

_Label `T: bug` added by @vireshk on 2021-05-28 07:56_

---

_Referenced in [rust-vmm/vhost-device#1](../../rust-vmm/vhost-device/pulls/1.md) on 2021-05-28 07:57_

---

_Comment by @pksunkara on 2021-05-28 08:01_

Duplicate of #1569.

Hello, you marked that you searched other issues. Can you please explain how you did not find this one?

---

_Closed by @pksunkara on 2021-05-28 08:01_

---

_Comment by @vireshk on 2021-05-28 08:54_

> Hello, you marked that you searched other issues. Can you please explain how you did not find this one?

I was able to find it earlier and somehow it looked to me that it would be fixed by clap 2.33.3, maybe I misread then..

But even after following #1569 I am not able to understand how to fix this problem. Do I understand correctly that it is only fixed for clap 3 onwards and not for 2.33.* ?

Thanks.

---

_Comment by @vireshk on 2021-05-28 09:13_

> But even after following #1569 I am not able to understand how to fix this problem. Do I understand correctly that it is only fixed for clap 3 onwards and not for 2.33.* ?

Ahh, I was able to get over it. Thanks.

clap = { version = "3.0.0-beta.2", features = ["yaml"] }


---

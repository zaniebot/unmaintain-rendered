---
number: 2313
title: " clap_version function built in shadow-rs adapt clap crates"
type: issue
state: closed
author: baoyachi
labels:
  - A-docs
assignees: []
created_at: 2021-01-27T12:27:21Z
updated_at: 2021-01-27T15:08:33Z
url: https://github.com/clap-rs/clap/issues/2313
synced_at: 2026-01-07T13:12:19-06:00
---

#  clap_version function built in shadow-rs adapt clap crates

---

_Issue opened by @baoyachi on 2021-01-27 12:27_

## clap_version function built in shadow-rs
```rust
fn main() {
    App::new("example_clap")
        .version(build::clap_version().as_str())
        .get_matches(); //USAGE: ./example_clap -V
}
```

## clap_version example
link:[clap_version demo](https://github.com/baoyachi/shadow-rs/blob/2915a18675cc297d9ea5a46d65c47d98097fae67/example_shadow/src/main.rs#L9-L11)

if you want use clap `version()`, also consider [https://github.com/baoyachi/shadow-rs](https://github.com/baoyachi/shadow-rs) integrating in you rust project.

I think shadow-rs `clap_version()`function  and `clap` `version()` function are a perfect match.


---

_Label `C: docs` added by @baoyachi on 2021-01-27 12:27_

---

_Comment by @ldm0 on 2021-01-27 13:12_

Clap already have this kind of ability. Check the [`crate_version`](https://docs.rs/clap/2.33.3/clap/macro.crate_version.html).

---

_Comment by @pksunkara on 2021-01-27 15:08_

Hello,

We really appreciate your enthusiasm to contribute to this open source project and to help make it better. To make use of the maintainers' time more productively, we have set up a few issue templates that should be used when creating an issue.

We see that you made an accidental mistake because the issue you created doesn't follow the template. We will be closing this until that is rectified. Please do so and message here and we will gladly re-open this.

Thanks

---

_Closed by @pksunkara on 2021-01-27 15:08_

---

---
number: 778
title: "Feature: cargo env simplified"
type: issue
state: closed
author: ishitatsuyuki
labels:
  - C-enhancement
assignees: []
created_at: 2016-12-17T07:26:06Z
updated_at: 2018-08-02T03:29:58Z
url: https://github.com/clap-rs/clap/issues/778
synced_at: 2026-01-10T01:26:35Z
---

# Feature: cargo env simplified

---

_Issue opened by @ishitatsuyuki on 2016-12-17 07:26_

This is a proposal of a more clean wrapper around Cargo.toml.

Right now the macro only supports version and author. If all things supported (package name, description) are taken into place, this would be cool.

Macro proposal: a wrapper around clap_app
```rust
clap_cargo_app!(
    // package name, version, authors, descriptions are taken into place, if applicable
    (@arg ...)
)
```

---

_Referenced in [clap-rs/clap#638](../../clap-rs/clap/issues/638.md) on 2016-12-17 07:29_

---

_Comment by @kbknapp on 2016-12-18 20:29_

Great idea! I think this would require each of those be implemented as separate macros, but you're right, I'd also like a single macro to pull in everything. Let me mull over the implementation over the holidays and I'll post back here.

The only thing I don't really want to do is have a `clap_app!` and `clap_cargo_app!` macro. I'd like to either combine these into the `clap_app!` macro, or do something like a `app_with_defaults!` macro.

i.e.

```rust
let app = app_from_crate!()
    // continue builder like normal
```

The above being the same as

```rust
let app = App::new(crate_name!())
    .version(crate_version!())
    .author(crate_authors!())
    .about(crate_description!())
    // other builders like normal.
```

---

_Label `C: macros` added by @kbknapp on 2016-12-18 20:30_

---

_Label `D: intermediate` added by @kbknapp on 2016-12-18 20:30_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-18 20:30_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-18 20:30_

---

_Label `T: new feature` added by @kbknapp on 2016-12-18 20:30_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-18 20:30_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-27 04:38_

---

_Comment by @kbknapp on 2016-12-31 05:28_

## ToDo

- [x] Add `crate_description!`
 - [x] `crate_description!` docs
- [x] Add `crate_name!`
 - [x] `crate_name!` docs
- [x] Add `app_from_crate!`
 - [x] `app_from_crate!` docs

---

_Referenced in [clap-rs/clap#798](../../clap-rs/clap/pulls/798.md) on 2016-12-31 13:20_

---

_Closed by @homu on 2016-12-31 22:04_

---

_Comment by @kbknapp on 2016-12-31 22:05_

Big thanks to @nabijaczleweli for implementing this 👍 

---

_Comment by @nabijaczleweli on 2017-01-01 00:58_

I aim to please

---

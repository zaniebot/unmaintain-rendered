---
number: 783
title: "Request: Add a crate_feature to disable macros that depend on cargo env vars"
type: issue
state: closed
author: acmcarther
labels:
  - C-enhancement
  - M-breaking-change
assignees: []
created_at: 2016-12-24T05:55:24Z
updated_at: 2018-08-02T03:29:58Z
url: https://github.com/clap-rs/clap/issues/783
synced_at: 2026-01-07T13:12:19-06:00
---

# Request: Add a crate_feature to disable macros that depend on cargo env vars

---

_Issue opened by @acmcarther on 2016-12-24 05:55_

The [crate_*! macros](https://github.com/kbknapp/clap-rs/blob/83bacb87bb854be8b559f9e9c4bda5c3f47a0489/src/macros.rs#L389) make a fairly valid assumption that consumers use `cargo` to build Rust projects by expecting certain environment variables to be present. Unfortunately, I am one of the very few not using `Cargo`. I'm actually experimenting with [Bazel](https://www.bazel.io/) to build my Rust projects -- but `clap` fails to compile because bazel is not providing the expected environment variables.

As of posting, the macros causing me issues are "crate_version!" and "crate_authors!", which seem to be provided as a convenience to `clap` users and aren't used internally.

If this proposal is rejected, I do have mitigation options -- faking those values or patching clap locally -- so please reject if you feel that not enough users are impacted.

Thanks!


---

_Comment by @kbknapp on 2016-12-27 04:23_

Wow, I wasn't aware of Bazel, that's interesting. 

Although I'm not against adding these to a feature, and simply adding said feature to the defaults but I *do* worry that there are people who have opted out of `default-features` and are using `crate_*!` macros. The problem is that would potentially be a silent breaking change.

Because of that concern, I think the best way forward is to add this to the 3x timeline, which hopefully isn't too far off (waiting on Macros 1.1).

I believe all you should have to do on your end is export those environmental variables while building, and it should work?

---

_Label `C: macros` added by @kbknapp on 2016-12-27 04:23_

---

_Label `D: easy` added by @kbknapp on 2016-12-27 04:23_

---

_Label `E: breaking change` added by @kbknapp on 2016-12-27 04:23_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-27 04:23_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-27 04:23_

---

_Label `W: 3.x` added by @kbknapp on 2016-12-27 04:23_

---

_Added to milestone `3.0` by @kbknapp on 2016-12-27 04:34_

---

_Comment by @acmcarther on 2016-12-27 04:39_

No worries, thanks for the quick response! Exporting fake values during my build step solves my problem and is no trouble for any length of time.

---

_Comment by @nabijaczleweli on 2016-12-27 12:18_

> do worry that there are people who have opted out of default-features and are using crate_*! macros.

Valid, but unnecessary concern. Consider:

```toml
[features]
no_cargo = []
```

Then:

```rust
#[cfg(not(feature="no_cargo"))]
macro_rules! crate_authors {
  // magic
}
```

That way you need an explicit opt-in to remove the macros, granting full backwards compatibility

---

_Referenced in [clap-rs/clap#786](../../clap-rs/clap/pulls/786.md) on 2016-12-27 12:34_

---

_Closed by @kbknapp on 2016-12-28 02:08_

---

_Referenced in [rust-lang/rust-roadmap-2017#12](../../rust-lang/rust-roadmap-2017/issues/12.md) on 2017-02-07 18:08_

---

_Comment by @jsgf on 2017-02-10 16:07_

This might cover the same territory: https://github.com/rust-lang/rust/issues/39656

---

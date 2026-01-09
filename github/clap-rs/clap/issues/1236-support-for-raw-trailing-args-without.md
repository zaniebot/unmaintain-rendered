---
number: 1236
title: "Support for raw trailing args without `--`"
type: issue
state: closed
author: bjgill
labels: []
assignees: []
created_at: 2018-03-28T21:37:08Z
updated_at: 2023-08-08T02:39:36Z
url: https://github.com/clap-rs/clap/issues/1236
synced_at: 2026-01-07T13:12:19-06:00
---

# Support for raw trailing args without `--`

---

_Issue opened by @bjgill on 2018-03-28 21:37_

I've been trying to switch `cargo clippy` over to use clap (https://github.com/rust-lang-nursery/rust-clippy/pull/2557), and have discovered that it doesn't quite work.

The reason is that cargo-clippy is just a thin wrapper around cargo-rustc. cargo-clippy has a small number of its own options (e.g. `--all`), and just passes the rest verbatim to cargo rustc.

i.e.
```
cargo clippy               => cargo rustc
cargo clippy --all         => cargo rustc  (run multiple times)
cargo clippy --frozen      => cargo rustc --frozen
cargo clippy -- --D clippy => cargo rustc -- --D clippy
```

I have mostly been able to replicate this behaviour with `AppSettings::TrailingVarArg`. However, this has introduced the need for an additional `--` to all the args passed through to cargo-rustc. This means that the last example would become the entirely rational but nonetheless odd `cargo clippy -- -- -D clippy`. 

I think I'm therefore asking whether it would be possible to have some sort of `Arg::very_raw(self, bool)` function that provided the same functionality as `raw` but without requiring `--`?

---

_Referenced in [rust-lang/rust-clippy#2557](../../rust-lang/rust-clippy/pulls/2557.md) on 2018-03-28 21:46_

---

_Comment by @kbknapp on 2018-03-30 01:43_

Try [`AppSettings::AllowMissingPositional`](https://docs.rs/clap/2.31.2/clap/enum.AppSettings.html#variant.AllowMissingPositional) and set [`Arg::allow_hyphen_values`](https://docs.rs/clap/2.31.2/clap/struct.Arg.html#method.allow_hyphen_values)

---

_Label `T: RFC / question` added by @kbknapp on 2018-03-30 01:43_

---

_Comment by @kbknapp on 2018-03-30 01:44_

The `AllowMissingPositional` is if you want to be able to short circuit to that final arg using `--` (but not *require* the `--` if you don't need it). If you don't care about short circuiting, just `allow_hyphen_values(true)` and `multiple(true)` should work.

---

_Comment by @kbknapp on 2018-04-01 03:34_

I'm going to close this, feel free to re-open if those settings don't do what you're looking for.

---

_Closed by @kbknapp on 2018-04-01 03:34_

---

_Referenced in [clap-rs/clap#5055](../../clap-rs/clap/issues/5055.md) on 2023-07-30 20:30_

---

_Comment by @RalfJung on 2023-07-30 20:30_

I think this is still a problem: https://github.com/clap-rs/clap/issues/5055

---

_Label `A-parsing` added by @epage on 2023-08-08 02:39_

---

_Label `E-medium` added by @epage on 2023-08-08 02:39_

---

_Label `A-parsing` removed by @epage on 2023-08-08 02:39_

---

_Label `E-medium` removed by @epage on 2023-08-08 02:39_

---

---
number: 2921
title: "Custom clap::Parser Crate Path"
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2021-10-22T22:24:50Z
updated_at: 2021-10-24T21:35:56Z
url: https://github.com/clap-rs/clap/issues/2921
synced_at: 2026-01-07T13:12:19-06:00
---

# Custom clap::Parser Crate Path

---

_Issue opened by @epage on 2021-10-22 22:24_

From https://github.com/TeXitoi/structopt/issues/339

> I have a lib that is using structopt and I would like to re-export it. Unfortunately it seems I can't do that because the macros are not reexported too:
> 
> In crate A:
> 
> ```rust
> pub use structopt::{StructOpt, clap};
> ```
> 
> In crate B:
> 
> ```rust
> use a::StructOpt;
> ```
> 
> But I get this compilation error:
> 
> ```
>    Compiling a v2.0.0 (...)
> error[E0433]: failed to resolve: could not find `structopt` in `{{root}}`
>   --> src/cli.rs:25:24
>    |
> 25 | #[derive(Clone, Debug, StructOpt)]
>    |                        ^^^^^^^^^ could not find `structopt` in `{{root}}`
> ```

StructOpt PR: https://github.com/TeXitoi/structopt/pull/506

---

_Label `T: enhancement` added by @epage on 2021-10-22 22:24_

---

_Label `C: derive macros` added by @epage on 2021-10-22 22:24_

---

_Renamed from "Custom Clap Crate Path" to "Custom clap::Parser Crate Path" by @epage on 2021-10-22 22:25_

---

_Comment by @pksunkara on 2021-10-22 23:39_

This is already possible in clap.

---

_Comment by @epage on 2021-10-23 00:43_

How so?  I've not seen anything like this in `clap_derive`?

---

_Comment by @pksunkara on 2021-10-24 21:35_

It's the avoidance of using `::clap` in the token stream we output (#2258). I am closing this but feel free to reopen if you find an issue.

---

_Closed by @pksunkara on 2021-10-24 21:35_

---

_Referenced in [clap-rs/clap#2258](../../clap-rs/clap/pulls/2258.md) on 2021-10-25 14:39_

---

_Referenced in [clap-rs/clap#2941](../../clap-rs/clap/pulls/2941.md) on 2021-10-25 14:46_

---

_Referenced in [clap-rs/clap#4414](../../clap-rs/clap/issues/4414.md) on 2022-10-22 16:17_

---

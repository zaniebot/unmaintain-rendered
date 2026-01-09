---
number: 5280
title: "Soundness: clap_lex makes incorrect assumption about OsStr"
type: issue
state: closed
author: Manishearth
labels: []
assignees: []
created_at: 2024-01-03T01:18:42Z
updated_at: 2024-02-08T17:15:23Z
url: https://github.com/clap-rs/clap/issues/5280
synced_at: 2026-01-07T13:12:20-06:00
---

# Soundness: clap_lex makes incorrect assumption about OsStr

---

_Issue opened by @Manishearth on 2024-01-03 01:18_

https://github.com/clap-rs/clap/blob/48d28aa689bfd0fb44ec025244b30ba261e2515a/clap_lex/src/ext.rs#L248-L256

It's not true that `OsStr` is _stably_ a transparent wrapper around `[u8]`, this code is technically unsound. It's fine as long as the Rust stdlib avoids making this change.

Fortunately the soon-to-be-stabilized [`as_os_str_bytes()`](https://doc.rust-lang.org/std/ffi/struct.OsStr.html#method.as_os_str_bytes) will fix this. Opening this issue to track it.

---

_Comment by @epage on 2024-01-04 19:17_

While I generally feel I'm not creative enough to play these games, when I analyzed this, I could not find a way that the language or standard library can change that would violate this assumption because of the roundtripping involved with the conversions: "ref to DST" (`&[u8]`) -> "ref to DST" (`&str`) -> "ref to DST" (`&OsStr`) -> "ref to DST" (`&str`) -> "ref to DST" (`&[u8]`).  Is there something I'm missing?

I guess if the language allowed refs to DSTs to choose their representation (`(ptr, ptr)` vs `(ptr, length)`) and they chose different representations, then this could happen but (again, maybe not being creative enough) I cannot think of a situation this would actually happen.

As you said, `as_os_str_bytes` will soon be in our MSRV and we can switch to it.  I'm more concerned about the perception of risk from people seeing this issue if the perception of risk doesn't match the reality of the risk.

---

_Comment by @Manishearth on 2024-01-04 19:54_

> Is there something I'm missing?

Yes, that it's still not a documented invariant; I'm in general not comfortable relying on those in unsafe code and usually note them when performing unsafe review. I mostly filed this issue so I have something to point to in the [published unsafe audit](https://github.com/google/rust-crate-audits/blob/7345370de0faff4a4812ad02653ffd3b6f31a9cf/audits.toml#L961-L970), it's not a huge deal and this will be a non-problem in the future anyway.

DSTs changing their representation is indeed a real worry. In general it's not a huge one: so much code relies on DST repr that it's unlikely to change without major warning, but that still does make it important to track. Fortunately in this case it will be fixed before any such change can come down the pipeline.

---

_Referenced in [clap-rs/clap#5344](../../clap-rs/clap/pulls/5344.md) on 2024-02-08 16:05_

---

_Comment by @epage on 2024-02-08 16:08_

> DSTs changing their representation is indeed a real worry. In general it's not a huge one: so much code relies on DST repr that it's unlikely to change without major warning, but that still does make it important to track. Fortunately in this case it will be fixed before any such change can come down the pipeline.

While I am fixing this (#4344), I'm wanting to understand this further as I find other needs for transmuting DSTs into each other (`const` functions).  Maybe I don't have a creative enough imagination but in what ways can the DST representation change where we can't transmute them between each other when the roundtripping guarantees exist like with `&str` / `&OsStr` / `&[u8]`.

---

_Closed by @epage on 2024-02-08 16:29_

---

_Comment by @Manishearth on 2024-02-08 16:56_

> Maybe I don't have a creative enough imagination but in what ways can the DST representation change where we can't transmute them between each other when the roundtripping guarantees exist like with `&str` / `&OsStr` / `&[u8]`.

The metadata order could swap. And the provenance could be weird.

---

_Comment by @epage on 2024-02-08 17:00_

So two DSTs in the same rustc version could have metadata in different order?  That seems odd.

---

_Comment by @Manishearth on 2024-02-08 17:15_

Rust has `randomize-type-layout` today and in the long run the hope seems to be that correctly written Rust code will always succeed under `randomize-type-layout`. I'm not sure if that option currently randomizes DST representations, but I don't think there's any reason for it not to in the long run.

So yes, that is a potential reality.

---

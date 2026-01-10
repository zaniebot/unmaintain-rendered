---
number: 5209
title: Unsafe code - Rust native support available now (?)
type: issue
state: closed
author: cylewitruk
labels: []
assignees: []
created_at: 2023-11-12T06:56:14Z
updated_at: 2023-11-13T15:36:49Z
url: https://github.com/clap-rs/clap/issues/5209
synced_at: 2026-01-10T01:28:08Z
---

# Unsafe code - Rust native support available now (?)

---

_Issue opened by @cylewitruk on 2023-11-12 06:56_

The comment in the following code block pointing to a [PR in `rust-lang`](https://github.com/rust-lang/rust/pull/95290), which seems to have been superseded by [merged PR 109698](https://github.com/rust-lang/rust/pull/109698), maybe warrants a fresh look at the usage of unsafe here?

https://github.com/clap-rs/clap/blob/3aeea916e8e426e8389357a67d9b8f9a1fceba3e/clap_lex/src/ext.rs#L272C24-L272C24

---

_Renamed from "Unsafe code block native support resolved (?)" to "Unsafe code - Rust native support available now (?)" by @cylewitruk on 2023-11-13 09:48_

---

_Comment by @epage on 2023-11-13 15:36_

I wrote rust-lang/rust#109698 based on my work within clap.    Our use of unsafe meets the new guidelines that will be stabilized this week.  We'll update to the new API when 1.74 becomes our MSRV.

---

_Closed by @epage on 2023-11-13 15:36_

---

_Referenced in [stacks-network/stacks-core#4045](../../stacks-network/stacks-core/issues/4045.md) on 2023-11-13 16:35_

---

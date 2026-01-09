---
number: 3828
title: Replace lazy_static with once_cell
type: issue
state: closed
author: jplatte
labels: []
assignees: []
created_at: 2022-06-14T13:57:51Z
updated_at: 2022-06-14T14:44:17Z
url: https://github.com/clap-rs/clap/issues/3828
synced_at: 2026-01-07T13:12:20-06:00
---

# Replace lazy_static with once_cell

---

_Issue opened by @jplatte on 2022-06-14 13:57_

Many other projects have switched from `lazy_static` to `once_cell` because it offers a macro-free solution to the same problem (with the only caveat being missing `no_std` support which I'm pretty sure is irrelevant for clap). Would you be open to a PR that does the same thing here?

In the future, the `once_cell` dependency can likely be removed in favor of [the same functionality in `std`](https://github.com/rust-lang/rust/issues/74465).

---

_Comment by @matklad on 2022-06-14 14:11_

>with the only caveat being missing no_std support

FWIW, the subset of the API which *can* be reasonably implented in no-std is available:

https://docs.rs/once_cell/latest/once_cell/race/index.html

The "I can't notify OS that I am blocked, so I am just going to spin" part is indeed not available, but I'd argue that is a feature :-) 

---

_Referenced in [clap-rs/clap#3829](../../clap-rs/clap/pulls/3829.md) on 2022-06-14 14:13_

---

_Comment by @epage on 2022-06-14 14:13_

Huh, I thought there was a reason why I had kept it but not seeing a reason to.  #3829 will switch us over.

---

_Comment by @jplatte on 2022-06-14 14:26_

> The "I can't notify OS that I am blocked, so I am just going to spin" part is indeed not available, but I'd argue that is a feature :-)

Hmm, I think that's what tracing wants.. (https://github.com/tokio-rs/tracing/issues/1165#issuecomment-752718278) 😕

I guess that also means https://github.com/matklad/once_cell/issues/61 should be closed? In any case that issue is probably the place to continue this conversation.

---

_Closed by @epage on 2022-06-14 14:44_

---

_Referenced in [xiph/rav1e#2962](../../xiph/rav1e/pulls/2962.md) on 2022-07-04 17:14_

---

_Referenced in [BurntSushi/bstr#124](../../BurntSushi/bstr/issues/124.md) on 2022-09-02 15:27_

---

_Referenced in [mindvalley/wukong-cli#50](../../mindvalley/wukong-cli/pulls/50.md) on 2023-01-04 05:07_

---

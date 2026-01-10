---
number: 2076
title: Consider yanking Clap 2.9.2
type: issue
state: closed
author: matklad
labels:
  - C-bug
assignees: []
created_at: 2020-08-15T12:05:43Z
updated_at: 2020-08-18T07:35:18Z
url: https://github.com/clap-rs/clap/issues/2076
synced_at: 2026-01-10T01:27:12Z
---

# Consider yanking Clap 2.9.2

---

_Issue opened by @matklad on 2020-08-15 12:05_

Hi!

Clap 2.9.2 (and maybe some other versions, I haven't done an investigation) triggers future compatablity warning in this code:

https://github.com/clap-rs/clap/blob/9605ea83aab50113fcd190e9a54adfaae1634072/src/macros.rs#L506

This is an erroneous definition of a macro, because `$ident` fragment lacks `:ident` specifier. This has been a deny-by-default lint for some time, and we'd want to hard error it eventually (and most likely rather soon).  So, given that this crate might stop to compile eventually, it seems prudent to yank it, to give reverse-dependeices a heads up!

cc https://github.com/rust-lang/rust/pull/75516
cc https://github.com/dzamlo/treeify/pull/2

---

_Label `T: bug` added by @matklad on 2020-08-15 12:05_

---

_Renamed from "Consider yanking Clap 2.9" to "Consider yanking Clap 2.9.2" by @matklad on 2020-08-15 12:34_

---

_Comment by @CreepySkeleton on 2020-08-15 18:33_

Sure. It's hard to tell for sure which versions are affected because not every version has a tag I could checkout, so I just located the earliest bug-free version - which is `2.21.1` - and yanked everything in between. At the end of the day, they are all are just a history; there are more than 10 minor releases after that point.

I didn't spend any time on checking older versions because they're quite ancient and I doubt anyone will ever notice. Anyway, whoever cares is free to contact us and we'll yank them as well.

---

_Closed by @CreepySkeleton on 2020-08-15 18:33_

---

_Comment by @pksunkara on 2020-08-15 18:45_

This has been there since v1.4.0. Do we want to yank all of them? I am not sure why we are giving importance to yanking. What if people prefer an older version of rust?

Also, the usage of clap by versions (atleast from public crates) is [here](https://lib.rs/crates/clap/rev)

---

_Comment by @CreepySkeleton on 2020-08-15 18:59_

I don't mind, go ahead if you want to.

`1.4.0` was released five years ago. If those people are so dead set, they can just put the desired version in `Cargo.lock` manually (and it's probably already there). Yanking doesn't remove the crate from crates.io, it just prevents _new_ crates from depending on it.

> Also, the usage of clap by versions (atleast from public crates) is here

Just as expected: the ~90% peak at the latest minor version and the _loong_ low tail of older deps. I'd say we don't care. A curious phenomena: local peaks on "terminator versions" (i.e `2.23.3` when the next version is `2.24.x`).

---

_Reopened by @Dylan-DPC-zz on 2020-08-16 00:11_

---

_Comment by @Dylan-DPC-zz on 2020-08-16 00:11_

Reopening this because as per how the process goes with similar advisories, the versions have to be yanked 

---

_Referenced in [gluon-lang/gluon_language-server#35](../../gluon-lang/gluon_language-server/issues/35.md) on 2020-08-16 02:31_

---

_Comment by @pksunkara on 2020-08-16 07:51_

So, should I yank from 1.4.0 onwards?

---

_Referenced in [rust-lang/rust#75516](../../rust-lang/rust/pulls/75516.md) on 2020-08-17 07:24_

---

_Comment by @CreepySkeleton on 2020-08-18 07:35_

Yanked, God bless dumb python scripts. Why doesn't cargo have the "yank everything from X.X.X to Y.Y.Y" functionality?

---

_Closed by @CreepySkeleton on 2020-08-18 07:35_

---

_Referenced in [rust-lang/rust#128425](../../rust-lang/rust/pulls/128425.md) on 2024-08-14 06:03_

---

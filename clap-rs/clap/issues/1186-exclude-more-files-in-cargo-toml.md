---
number: 1186
title: Exclude more files in Cargo.toml
type: issue
state: closed
author: RazrFalcon
labels:
  - C-enhancement
  - E-medium
  - A-meta
  - E-easy
assignees: []
created_at: 2018-02-18T17:50:30Z
updated_at: 2018-08-02T03:30:19Z
url: https://github.com/clap-rs/clap/issues/1186
synced_at: 2026-01-10T01:26:45Z
---

# Exclude more files in Cargo.toml

---

_Issue opened by @RazrFalcon on 2018-02-18 17:50_

Exclusion of `CHANGELOG.md`, `CONTRIBUTORS.md` and `README.md` will decrease `.crate` file size from 186KiB to 133KiB.

---

_Comment by @kbknapp on 2018-02-19 04:53_

Great catch, thanks @RazrFalcon! [I actually started doing this in v3](https://github.com/kbknapp/clap-rs/blob/bd08e73e5459600db2646143dc4367690035dab7/Cargo.toml#L6-L19), but didn't backport it to v2.

If someone wants an easy PR this is a great one. If it hasn't been knocked out by the time I make the next merge I'll knock it out.

---

_Label `T: enhancement` added by @kbknapp on 2018-02-19 04:54_

---

_Label `D: easy` added by @kbknapp on 2018-02-19 04:54_

---

_Label `P3: want to have` added by @kbknapp on 2018-02-19 04:54_

---

_Label `W: 2.x` added by @kbknapp on 2018-02-19 04:54_

---

_Label `M: mentored` added by @kbknapp on 2018-02-19 04:54_

---

_Label `C: meta` added by @kbknapp on 2018-02-19 04:54_

---

_Label `good first issue` added by @kbknapp on 2018-02-19 04:54_

---

_Comment by @kbknapp on 2018-02-19 05:00_

*Slightly* on a different note, I wonder if anyone has done any testing with removing all comments during `cargo package`, I'd imagine that'd save quite a bit community wide. Considering clap is nearly 45% comments (counting `src/` dir only and only counting lines). And comment lines I'd imagine are far more dense than code.

```
kevin@beefcake: ~/Projects/clap-rs 
➜ tokei src
-------------------------------------------------------------------------------
 Language            Files        Lines         Code     Comments       Blanks
-------------------------------------------------------------------------------
 Rust                   40        19373        10852         7468         1053
-------------------------------------------------------------------------------
 Total                  40        19373        10852         7468         1053
-------------------------------------------------------------------------------
```

---

_Comment by @kbknapp on 2018-02-19 05:18_

In my unscientific test, doing *both* more excludes (`*.md`) and removing all comments brings it down to 68K. I'd say that's decently substantial (basically 33% of the original size).

---

_Comment by @kbknapp on 2018-02-19 05:23_

Hmm, actually excluding the `README.md` might be a bad idea. This is what is rendered on the crates.io package page :\

---

_Comment by @kbknapp on 2018-02-19 05:37_

I'm not sure any more...partially because it's late, but also looking through the cargo and crates.io source, it looks like the readme *may* get uploaded separately...but to be safe we should ask someone from the cargo or crates.io team to confirm.

---

_Comment by @RazrFalcon on 2018-02-19 10:25_

>This is what is rendered on the crates.io package page

I thought that it uses github repo. My bad.

The problem with `.crate` files is that they can contain anything, but only `src` dir and `Cargo.toml` are important (afaiu). There are no official guidelines about it.

But if, for some reasons, original repo will disappear - this is the only way of retrieving the source code, so at least `tests` should still be present.

Have no idea what should be done in this case, but currently we just decreasing the bandwidth and bloating the `.cargo` dir (last time I've check it was 3GiB).

---

_Referenced in [rust-lang/cargo#4644](../../rust-lang/cargo/pulls/4644.md) on 2018-03-04 03:50_

---

_Referenced in [rust-cli/team#8](../../rust-cli/team/issues/8.md) on 2018-03-05 21:05_

---

_Referenced in [rust-cli/team#20](../../rust-cli/team/issues/20.md) on 2018-03-20 11:24_

---

_Label `W: 2.x` removed by @kbknapp on 2018-07-22 02:54_

---

_Comment by @kbknapp on 2018-07-22 02:54_

This has started on v3-master. I'm going to close this issue as I don't plan on backporting to v2 and any further work would need some official Rust guidelines on the matter.

---

_Closed by @kbknapp on 2018-07-22 02:54_

---

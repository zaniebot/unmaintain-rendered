---
number: 1155
title: "Don't compile ansi_term on Windows"
type: issue
state: closed
author: XAMPPRocky
labels:
  - C-enhancement
assignees: []
created_at: 2018-01-27T17:26:15Z
updated_at: 2018-08-02T03:30:17Z
url: https://github.com/clap-rs/clap/issues/1155
synced_at: 2026-01-07T13:12:19-06:00
---

# Don't compile ansi_term on Windows

---

_Issue opened by @XAMPPRocky on 2018-01-27 17:26_

`ansi_term` currently doesn't work on [32bit Windows platforms](https://github.com/ogham/rust-ansi-term/issues/31) and is not being maintained. `clap` being dependent by so many crates(mine included) needs to resolve this since there is no maintainer for `ansi_term`. There are two solutions I can think of.

- `clap` maintains a forked version of `ansi_term`.
- `clap` pulls required functionality of our `ansi_term` and into it's own module in tree.


---

_Comment by @SergioBenitez on 2018-01-30 22:33_

Alternatively, migrate to [`yansi`](https://docs.rs/yansi/), a crate I created for Rocket. The API is fairly similar to `ansi_term`'s, so migrating should be very simple. If there's a feature missing in `yansi` that makes migrating difficult, I'm happy to adjust the API.

---

_Comment by @BurntSushi on 2018-01-30 22:56_

@SergioBenitez Your crate only supports ANSI escape sequences, which won't work in Windows consoles that don't have ANSI support. (Your API is also appears fundamentally incompatible with how Windows console coloring works.)

---

_Comment by @SergioBenitez on 2018-01-30 22:59_

@BurntSushi Yes, I'm quite aware of that; it's even part of the crate's name! This issue is about moving away from `ansi_term`, which _also_ doesn't support older Windows consoles.

---

_Comment by @BurntSushi on 2018-01-30 23:03_

@SergioBenitez Let me rephrase: It would be great to move to a library that supports Windows console coloring.

---

_Comment by @kbknapp on 2018-01-31 01:35_

@Aaronepower `ansi_term` [isn't used by `clap` on Windows](https://github.com/kbknapp/clap-rs/blob/969c3adb4107d7e2934d2721f8eda9992985301d/src/fmt.rs#L142-L153), as I just default to no color support on Windows. However, the plan is to move to [`termcolor`](https://github.com/BurntSushi/ripgrep/tree/master/termcolor) to enable color support for Windows. Progress can be tracked specifically in #836 

However, this color re-write will be implemented in v3 (which actively being worked again after a stagnant period) and can be tracked via the overall v3 tracking issue #1037 

I've implemented a feature freeze for v2 so I can actually get v3 out the door. So it shouldn't be long before v3-alpha1 is out.

I'm going to close this so comments can be consolidated in one place. I appreciate the suggestions @SergioBenitez and if `termcolor` ends up not working out and I have to default back to no Windows color support, but still want color support elsewhere I'll look into swapping `ansi_term` with `yansi`.

---

_Closed by @kbknapp on 2018-01-31 01:35_

---

_Comment by @XAMPPRocky on 2018-01-31 12:19_

@kbknapp [It's still compiled on Windows](https://ci.appveyor.com/project/Aaronepower/tokei/build/1.0.264/job/9m5yfof69111sqdv#L124) which is the real problem.

---

_Comment by @kbknapp on 2018-01-31 18:12_

@Aaronepower Ah, I see my mistake. I'll re-open this issue as "Don't compile `ansi_term` on Windows which should be [totally possible with `cargo`](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#platform-specific-dependencies).

---

_Reopened by @kbknapp on 2018-01-31 18:12_

---

_Renamed from "Move away from using ansi_term." to "Don't compile ansi_term on Windows" by @kbknapp on 2018-01-31 18:12_

---

_Label `D: easy` added by @kbknapp on 2018-01-31 18:13_

---

_Label `P3: want to have` added by @kbknapp on 2018-01-31 18:13_

---

_Label `M: mentored` added by @kbknapp on 2018-01-31 18:13_

---

_Label `good first issue` added by @kbknapp on 2018-01-31 18:13_

---

_Comment by @kbknapp on 2018-01-31 18:13_

I'd be willing to merge a PR for 2.29.3 that fixes this.

---

_Added to milestone `2.29.3` by @kbknapp on 2018-02-02 01:50_

---

_Comment by @kbknapp on 2018-02-04 23:08_

Turns out this is harder than I had first thought: https://github.com/rust-lang/cargo/issues/1197



---

_Removed from milestone `2.29.3` by @kbknapp on 2018-02-04 23:08_

---

_Label `W: postponed` added by @kbknapp on 2018-02-04 23:09_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-04 23:09_

---

_Label `D: easy` removed by @kbknapp on 2018-02-04 23:09_

---

_Label `M: mentored` removed by @kbknapp on 2018-02-04 23:09_

---

_Label `good first issue` removed by @kbknapp on 2018-02-04 23:09_

---

_Label `W: blocking on external` added by @kbknapp on 2018-02-04 23:10_

---

_Label `W: postponed` removed by @kbknapp on 2018-02-04 23:10_

---

_Comment by @kbknapp on 2018-02-12 20:03_

For those compiling *only* to windows, not using the `color` feature will disable compiling `ansi_term`

```
kevin@beefcake: ~/Projects/clap-rs 
➜ cargo build --no-default-features --features wrap_help,suggestions
   Compiling strsim v0.7.0
   Compiling libc v0.2.36
   Compiling bitflags v1.0.1
   Compiling unicode-width v0.1.4
   Compiling term_size v0.3.1
   Compiling textwrap v0.9.0
   Compiling clap v2.29.4 (file:///home/kevin/Projects/clap-rs)
    Finished dev [unoptimized + debuginfo] target(s) in 8.0 secs
```

I'm going to close this for now, and re-address once cargo supports per-target deps. I'll also keep this in mind during the great color redux in v3.

---

_Closed by @kbknapp on 2018-02-12 20:03_

---

_Comment by @retep998 on 2018-02-12 20:47_

>re-address once cargo supports per-target deps

You mean like `[target.i686-pc-windows-gnu.dependencies]` or `[target.'cfg(all(windows, target_arch="x86"))'.dependencies]`? Or did you mean per-target features?

---

_Comment by @kbknapp on 2018-02-12 20:59_

@retep998 I meant them somewhat interchangeably. The issue being the `color` feature is currently ignored on Windows, yet even though it's ignored the `ansi_term` dep is still compiled.

What I'd like to happen is per-target features (so that on everything but Windows the `color` feature will include `ansi_term`, but on Windows the `color` feature will simply be empty, thus not compiling `ansi_term` for no reason). 

If this is already possible, I may have misunderstood the state of things.

---

_Comment by @retep998 on 2018-02-12 21:01_

While features themselves cannot be target specific, dependencies can, even if they are optional. You can enable the feature from any target, but the dependency will only be compiled for the targets it specifies.

---

_Comment by @kbknapp on 2018-02-12 21:14_

TIL...just tested it and it works!

Wow 🎉 Thanks for the explanation @retep998 👍 

I'll re-open this and put in the PR.

---

_Reopened by @kbknapp on 2018-02-12 21:14_

---

_Referenced in [clap-rs/clap#1178](../../clap-rs/clap/pulls/1178.md) on 2018-02-12 21:18_

---

_Label `W: 3.x` removed by @kbknapp on 2018-02-12 22:47_

---

_Label `W: blocking on external` removed by @kbknapp on 2018-02-12 22:47_

---

_Label `T: enhancement` added by @kbknapp on 2018-02-12 22:47_

---

_Label `E: optional dep` added by @kbknapp on 2018-02-12 22:47_

---

_Label `W: 2.x` added by @kbknapp on 2018-02-12 22:47_

---

_Closed by @kbknapp on 2018-02-13 01:52_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-13 02:05_

---

_Referenced in [clap-rs/clap#1182](../../clap-rs/clap/pulls/1182.md) on 2018-02-13 21:49_

---

_Referenced in [volks73/cargo-wix#45](../../volks73/cargo-wix/issues/45.md) on 2018-04-15 19:46_

---

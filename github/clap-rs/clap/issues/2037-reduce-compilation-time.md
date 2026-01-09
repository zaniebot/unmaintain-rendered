---
number: 2037
title: Reduce compilation time
type: issue
state: closed
author: CreepySkeleton
labels:
  - C-enhancement
  - A-meta
  - S-waiting-on-mentor
assignees: []
created_at: 2020-07-28T11:47:27Z
updated_at: 2025-01-28T17:21:59Z
url: https://github.com/clap-rs/clap/issues/2037
synced_at: 2026-01-07T13:12:19-06:00
---

# Reduce compilation time

---

_Issue opened by @CreepySkeleton on 2020-07-28 11:47_

[All features](https://gistpreview.github.io/?8ee8a91bfe081221672908eb57fcf723)
[No features](https://gistpreview.github.io/?2e3e54008f987749cd1cbf31f8f1f0e9)

The `clap` crate (not counting dependencies) takes about 11 seconds to compile. All dependencies put together take roughly the same amount of time.

I have a few thoughts on how to improve the situation. Generally, compile time wins these days are accomplished through "compile less code", hence:
- [ ] Feature-gate `#[derive(Clone, Copy, Eq...]`. The features should probably be fine-grained, like `impl-traits-eq` enables `Eq + PartialEq`. Plus the "enable everything" `impl-traits` feature that implies all the traits.
  - [ ] Avoid using derives and use some sort of `STRUCT!` macro as shown [here](https://github.com/retep998/winapi-rs/issues/218#issuecomment-145988469). It turns out that even built-in derives have impact on compilation time when you have lots of them, and _every_ struct here has `#[derive(Debug)]`.
- [x] Optimize `proc-macro-error` (drop `syn-mid` dep). I had already done that but decided to play around a bit more and then forgot about it. Oops.
- [ ] Make clap modular so developers could choose exactly the set of features they need and avoid compile the others. I'll crate a separate issue a bit later.

The list is probably not exhaustive...

---

_Label `T: RFC / question` added by @CreepySkeleton on 2020-07-28 11:47_

---

_Label `C: meta` added by @CreepySkeleton on 2020-07-28 11:47_

---

_Label `C: other` added by @CreepySkeleton on 2020-07-28 11:47_

---

_Referenced in [clap-rs/clap#2038](../../clap-rs/clap/pulls/2038.md) on 2020-07-29 02:13_

---

_Comment by @mainrs on 2020-07-30 11:31_

Hey!
If you want I can tackle down the trait implementation features :) How fine-grained do you want it to be though? 

---

_Comment by @CreepySkeleton on 2020-07-31 12:24_

Sure, you can try, but keep in mind that the real goal is to reduce compilation time. If the measurements will show no real difference (at least half second, everything below the threshold is noise), I will say "well we tried" and move on to other ideas.

---

_Comment by @mainrs on 2020-07-31 13:35_

Sure thing! Could you tell me how to managed to time the compilation? And create the visuals for it as well :)

---

_Comment by @CreepySkeleton on 2020-07-31 14:23_

The invocation was 
```sh
cargo clean # make sure the we build from scratch
cargo +nightly build -Z timings -p clap:3.0.0-beta.1 --features "yaml unstable"
```

This will create `cargo-timing-<HASH>.html` file that you can open in your web browser and see the adorable stats you were so excited about :) Then I uploaded the raw html on `gist.github.com` and linked it through `gistpreview.github.io` so you see the rendered version instead of raw HTML guts. 

---

_Referenced in [epage/clapng#161](../../epage/clapng/issues/161.md) on 2021-12-06 20:17_

---

_Label `C: other` removed by @epage on 2021-12-08 20:18_

---

_Label `T: RFC / question` removed by @epage on 2021-12-08 21:00_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:00_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-10 21:59_

---

_Referenced in [clap-rs/clap#4065](../../clap-rs/clap/pulls/4065.md) on 2022-08-11 19:17_

---

_Referenced in [clap-rs/clap#4096](../../clap-rs/clap/pulls/4096.md) on 2022-08-19 16:55_

---

_Referenced in [clap-rs/clap#4114](../../clap-rs/clap/pulls/4114.md) on 2022-08-25 18:44_

---

_Referenced in [clap-rs/clap#4191](../../clap-rs/clap/issues/4191.md) on 2022-09-07 19:29_

---

_Comment by @mgeisler on 2022-09-14 09:24_

> The `clap` crate (not counting dependencies) takes about 11 seconds to compile

Hi @CreepySkeleton, I suppose this is a "cold" compilation of a small Hello World-style program which depends on Clap? As a developer, I would be interested in the _incremental_ compilation time when I change my application by, e.g., adding a new command line flag.

Could you perhaps measure that as well?

---

_Referenced in [clap-rs/clap#4218](../../clap-rs/clap/issues/4218.md) on 2022-09-15 15:14_

---

_Referenced in [clap-rs/clap#4679](../../clap-rs/clap/issues/4679.md) on 2023-01-27 19:41_

---

_Referenced in [clap-rs/clap#4682](../../clap-rs/clap/issues/4682.md) on 2023-02-23 21:23_

---

_Referenced in [clap-rs/clap#4793](../../clap-rs/clap/issues/4793.md) on 2023-03-27 18:20_

---

_Referenced in [clap-rs/clap#4956](../../clap-rs/clap/issues/4956.md) on 2023-06-09 15:13_

---

_Referenced in [clap-rs/clap#5657](../../clap-rs/clap/issues/5657.md) on 2024-08-10 12:58_

---

_Comment by @epage on 2025-01-28 17:21_

I'm going to go ahead and close this.  While this is still a priority, we've taken care of a lot of low hanging fruit.  If we people have ideas for further improvements, we should create specific issues/PRs for those.

---

_Closed by @epage on 2025-01-28 17:21_

---

---
number: 2691
title: "clap `2.33.3` has incorrect MSRV due to bitflags `1.3` MSRV bump"
type: issue
state: closed
author: CosmicHorrorDev
labels:
  - A-docs
assignees: []
created_at: 2021-08-13T20:31:24Z
updated_at: 2021-09-23T00:12:32Z
url: https://github.com/clap-rs/clap/issues/2691
synced_at: 2026-01-07T13:12:19-06:00
---

# clap `2.33.3` has incorrect MSRV due to bitflags `1.3` MSRV bump

---

_Issue opened by @CosmicHorrorDev on 2021-08-13 20:31_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Where?

https://github.com/bitflags/bitflags/issues/255

### What's wrong?

The README states that `clap` >=2.21 has an MSRV of 1.42, but the dependency on `bitflags = "1.2"` allows for v1.3 which bumps the requirement up to 1.46

### How to fix?

Either pin the requirement for bitflags to 1.2.x, bump the MSRV, or work with bitflags to see if something else can be sorted out

---

_Label `C: docs` added by @CosmicHorrorDev on 2021-08-13 20:31_

---

_Referenced in [CosmicHorrorDev/vdf-rs#23](../../CosmicHorrorDev/vdf-rs/issues/23.md) on 2021-08-13 20:34_

---

_Comment by @epage on 2021-08-13 20:43_

This is one of those complications from MSRV:
- imo the version constraint is correct and we shouldn't be overly constraining  for the sake of MSRV.
- Other crates are allowed to advance their MSRV without it being a breaking change.
- However, the tools for supporting MSRV are still quite crude

The best I can offer at the moment is to use `-Z minimal-versions` or to patch the dependency.

---

_Closed by @pksunkara on 2021-08-13 20:43_

---

_Referenced in [clap-rs/clap#2735](../../clap-rs/clap/issues/2735.md) on 2021-08-23 13:24_

---

_Referenced in [clap-rs/clap#2734](../../clap-rs/clap/issues/2734.md) on 2021-08-23 13:29_

---

_Referenced in [lsd-rs/lsd#452](../../lsd-rs/lsd/pulls/452.md) on 2021-08-24 05:54_

---

_Referenced in [pop-os/system76-power#270](../../pop-os/system76-power/issues/270.md) on 2021-09-16 22:55_

---

_Comment by @rfan-debug on 2021-09-23 00:12_

@epage I ran into the same error. Would you mind giving me an example how to patch the transitive dependency `bitflags` of `clap`? My rustc version is `1.42.0`

I tried to use a patch as the following but it does not work.

```toml
[patch.crates-io]
clap = {git = "https://github.com/clap-rs/clap", tag = "v2.33.3"}

```


---

_Referenced in [foresterre/cargo-msrv#312](../../foresterre/cargo-msrv/issues/312.md) on 2022-02-22 15:06_

---

_Referenced in [lsd-rs/lsd#688](../../lsd-rs/lsd/pulls/688.md) on 2022-07-03 16:34_

---

_Referenced in [rust-lang/cargo#9930](../../rust-lang/cargo/issues/9930.md) on 2023-03-30 12:16_

---

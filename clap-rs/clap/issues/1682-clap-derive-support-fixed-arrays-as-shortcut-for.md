```yaml
number: 1682
title: "clap_derive: support fixed arrays as shortcut for number_of_values = N"
type: issue
state: open
author: CreepySkeleton
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2020-02-09T15:03:22Z
updated_at: 2022-11-08T05:15:37Z
url: https://github.com/clap-rs/clap/issues/1682
synced_at: 2026-01-12T16:14:11Z
```

# clap_derive: support fixed arrays as shortcut for number_of_values = N

---

_@CreepySkeleton_

Transferred from: https://github.com/TeXitoi/structopt/issues/349

It would be even better if fixed sized arrays or tuples with different value types would be supported directly:

```rust
#[derive(Clap)]
struct Args {
    #[clap(long)]
    range: [u32; N], // implies `required = true` and `number_of_values = N` 

    #[clap(long)]
    range: Option<[u32; N]>, // `number_of_values = N` 
}
```

---

_Label `C: derive macros` added by @CreepySkeleton on 2020-02-09 15:03_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-09 15:03_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-09 15:03_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-09 15:04_

---

_Comment by @pksunkara on 2020-03-02 17:22_

Could you create a separate issue for the tuples?

---

_Referenced in [TeXitoi/structopt#364](../../TeXitoi/structopt/issues/364.md) on 2020-03-25 18:08_

---

_Comment by @cecton on 2020-03-26 15:29_

Yeah actually having something automatic for tuples would solve my issue as I won't have to fight with Vec. (But I still believe the current impl of Vec is wrong but I didn't read your answer yet.)

---

_Referenced in [clap-rs/clap#1772](../../clap-rs/clap/issues/1772.md) on 2020-04-18 11:00_

---

_Comment by @CreepySkeleton on 2020-04-18 13:30_

This change would be breaking if implemented after 3.0 is out, so triaging for 3.0

---

_Removed from milestone `3.1` by @CreepySkeleton on 2020-04-18 13:30_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-04-18 13:30_

---

_Assigned to @ldm0 by @ldm0 on 2021-03-13 13:17_

---

_Renamed from "clap_derive: support fixed arrays as shortcur for number_of_values = N" to "clap_derive: support fixed arrays as shortcut for number_of_values = N" by @ldm0 on 2021-03-25 14:47_

---

_Label `W: 3.x` removed by @pksunkara on 2021-05-26 14:39_

---

_Removed from milestone `3.0` by @pksunkara on 2021-06-16 05:32_

---

_Unassigned @ldm0 by @pksunkara on 2021-06-16 05:32_

---

_Referenced in [epage/clapng#137](../../epage/clapng/issues/137.md) on 2021-12-06 19:11_

---

_Referenced in [epage/clapng#148](../../epage/clapng/issues/148.md) on 2021-12-06 19:16_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:15_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:15_

---

_Comment by @epage on 2021-12-09 20:40_

This also ties into
- https://github.com/clap-rs/clap/issues/2683
- #1717
- Existing key-value handling which we wouldn't want to break

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 20:41_

---

_Assigned to @ldm0 by @ldm0 on 2022-04-26 14:02_

---

_Referenced in [clap-rs/clap#3661](../../clap-rs/clap/pulls/3661.md) on 2022-04-26 14:04_

---

_Referenced in [clap-rs/clap#1717](../../clap-rs/clap/issues/1717.md) on 2022-08-19 18:59_

---

_Label `S-waiting-on-mentor` removed by @epage on 2022-11-08 05:15_

---

_Label `S-waiting-on-design` added by @epage on 2022-11-08 05:15_

---

_Comment by @epage on 2022-11-08 05:15_

I've pushed this back to design as we'd need to figure out how people can opt-out of this behavior in case a value parser wants to output one of these

---

_Referenced in [clap-rs/clap#4626](../../clap-rs/clap/issues/4626.md) on 2023-01-11 13:20_

---

_Referenced in [chinedufn/swift-bridge#138](../../chinedufn/swift-bridge/pulls/138.md) on 2023-01-14 17:40_

---

_Referenced in [mrozycki/rustmas#243](../../mrozycki/rustmas/pulls/243.md) on 2024-05-19 14:40_

---

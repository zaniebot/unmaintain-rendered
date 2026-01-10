---
number: 6118
title: "Mapping raw args to desired type on-the-fly similar to `serde_with::TryFromInto`"
type: issue
state: open
author: MrFoxPro
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2025-09-01T15:15:59Z
updated_at: 2025-09-02T13:45:06Z
url: https://github.com/clap-rs/clap/issues/6118
synced_at: 2026-01-10T01:28:22Z
---

# Mapping raw args to desired type on-the-fly similar to `serde_with::TryFromInto`

---

_Issue opened by @MrFoxPro on 2025-09-01 15:15_

### Clap Version

4.5.42

### Describe your use case

Due to limitations of clap, it's not always possible to get desired type from CLI: [example](https://github.com/clap-rs/clap/issues/2621).
Same thing applies to serde, but [there is convenient macro](https://docs.rs/serde_with/latest/serde_with/struct.TryFromInto.html) `serde_with::TryFromInto`, which allows in combination with `#[serde(flatten)]` to map values into desired type.

### Describe the solution you'd like

Something like `serde_with::TryFromInto`, but for clap.

### Additional Context

It's difficult to transform CLI input to different type, especially if there are multiple nested subcommands. Right now it requires a lot of boilerplate.

---

_Label `C-enhancement` added by @MrFoxPro on 2025-09-01 15:16_

---

_Renamed from "Mapping raw args to desired object on-the-fly similar to `serde_with::TryFromInto`" to "Mapping raw args to desired type on-the-fly similar to `serde_with::TryFromInto`" by @MrFoxPro on 2025-09-01 16:55_

---

_Comment by @epage on 2025-09-02 13:45_

It would help if you provided a concrete example, like code you tried but is failing.

There are two things I can think you mean:
- An individual argument type is unsupported.  We have the `value_parser` attribute for this.  To see what all conversions are supported for that, see https://docs.rs/clap/latest/clap/macro.value_parser.html.  That means we support `FromStr` but not `TryFrom` for failable operations though we do support `try_map` on other value parsers.
- A group of arguments (like #2621).  This requires implementing traits directly, see https://docs.rs/clap/latest/clap/_derive/index.html#mixing-builder-and-derive-apis

If its one of those cases, could you clarify why the current solutions aren't meeting your needs?

---

_Label `A-derive` added by @epage on 2025-09-02 13:45_

---

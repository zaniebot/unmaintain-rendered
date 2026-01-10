---
number: 3543
title: "`#[derive(ArgEnum)]` fails silently on enums with non-unit variants"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - E-easy
  - A-derive
assignees: []
created_at: 2022-03-07T16:45:58Z
updated_at: 2022-03-31T18:52:42Z
url: https://github.com/clap-rs/clap/issues/3543
synced_at: 2026-01-10T01:27:42Z
---

# `#[derive(ArgEnum)]` fails silently on enums with non-unit variants

---

_Issue opened by @epage on 2022-03-07 16:45_

### Discussed in https://github.com/clap-rs/clap/discussions/3533

<div type='discussions-op-text'>

<sup>Originally posted by **Shir0kamii** March  4, 2022</sup>
Hi!

I stumbled on a case where the derive implementation of ArgEnum does not produce its impl block without warning. It happens if the enum contains a non-unit variant.

If you try to compile code such as

```rust
#[derive(ArgEnum)]
pub enum Test {
    Foo,
    Bar,
    Baz(u16),
}
```

The derive doesn't produce any code, and errors are later emitted, reporting that Test does not implement ArgEnum. I would consider that as a bug, because it is confusing and unfriendly. It would be better if an error was emitted from the macro.

I think I found the [culprit](https://github.com/clap-rs/clap/blob/e937955efba002cbce317b715c151d359e9229a6/clap_derive/src/derives/arg_enum.rs#L37-L39) and would be willing to contribute a fix.

However, while browsing similar issues, I found [this discussion](https://github.com/clap-rs/clap/discussions/2561) where one of you proposed then implemented a `#[clap(skip)]` attribute for skipping variants in ArgEnum implementation. I think that an enum with a skipped non-unit variant should not trigger any error, and instead produce a valid implementation. If you approve of it, I would like to help on that as well.

What do you think?</div>

---

_Label `C-bug` added by @epage on 2022-03-07 16:45_

---

_Label `E-easy` added by @epage on 2022-03-07 16:45_

---

_Label `A-derive` added by @epage on 2022-03-07 16:45_

---

_Added to milestone `3.2` by @epage on 2022-03-07 16:46_

---

_Referenced in [clap-rs/clap#3591](../../clap-rs/clap/pulls/3591.md) on 2022-03-30 01:24_

---

_Comment by @Shir0kamii on 2022-03-31 18:23_

This can be closed since #3591 has been merged.

---

_Closed by @epage on 2022-03-31 18:52_

---

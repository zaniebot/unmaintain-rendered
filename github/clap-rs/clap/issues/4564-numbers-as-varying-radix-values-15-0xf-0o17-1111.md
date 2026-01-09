---
number: 4564
title: numbers as varying radix values, 15 0xF 0o17 1111
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-builder
  - S-wont-fix
assignees: []
created_at: 2022-12-20T02:43:54Z
updated_at: 2025-08-07T20:08:42Z
url: https://github.com/clap-rs/clap/issues/4564
synced_at: 2026-01-07T13:12:20-06:00
---

# numbers as varying radix values, 15 0xF 0o17 1111

---

_Issue opened by @epage on 2022-12-20 02:43_

### Discussed in https://github.com/clap-rs/clap/discussions/4563

<div type='discussions-op-text'>

<sup>Originally posted by **jtmoon79** December 19, 2022</sup>
tl;dr a nice feature for numbers would allow reading passed argument as differing radix among the traditional hexadecimal, decimal, octal, and binary.

I have [implemented this here](https://github.com/jtmoon79/super-speedy-syslog-searcher/blob/2157861027eff2cade51aa950a6a4300e86a1e50/src/bin/bin.rs#L533-L604). It's a good bit of work. Would be nice if `clap` handled this for me. I'm imagining an option like `allow_radix_all`

```rust
    #[clap(
        required = false,
        long,
        default_value_t = 0xFFFF,
        allow_radix_all,
    )]
    number: u64,
```

And perhaps, further fine-grained options would also be allowed: `allow_radix_hexadecimal`, `allow_radix_octal`, `allow_radix_binary`.  So `allow_radix_all` would just enable all the prior.
And might even want to throw in `disallow_radix_decimal` for some users that really want such.

Parsing could require a prepended `0` before the radix symbol, e.g. `0xF`, `0d15`, `0o17`, `0b1111` and reject without, e.g. `xF`, `d15`, `o17`, `b1111`. _or_ could be flexible and accept both. I'm not sure what is best there.</div>

---

_Label `C-enhancement` added by @epage on 2022-12-20 02:43_

---

_Label `A-derive` added by @epage on 2022-12-20 02:43_

---

_Label `S-waiting-on-design` added by @epage on 2022-12-20 02:43_

---

_Label `A-derive` removed by @epage on 2022-12-20 02:47_

---

_Label `A-builder` added by @epage on 2022-12-20 02:47_

---

_Comment by @epage on 2022-12-20 02:50_

There are a couple routes we can go with this
- Create wrapper types for a specific use case and have the derive/builder automatically pick it up, like in #4074
- Support this on the existing numeric value parsers

I somewhat lean towards supporting `0x`, `0o`, and `0b` prefixes on integer value parsers without any configuration but am still undecided.  One concern with this is the slipper slope for digit separators, ["human sizes"](https://docs.rs/human-size/0.4.2/human_size/), etc which I wouldn't want to bake all of that in.

---

_Label `S-waiting-on-design` removed by @epage on 2025-08-07 20:08_

---

_Label `S-wont-fix` added by @epage on 2025-08-07 20:08_

---

_Comment by @epage on 2025-08-07 20:08_

I'm leaning towards this being handled externally, like with `clio` for paths.

In addition to the questions on which prefixes, whether a prefix is required, separates, and units, having this built-in to the existing value parsers means it is present even if a user doesn't care about it, slowing down build times and bloating binaries.

I'm going to go ahead and close.  However, we can re-evaluate if someone comes up with some clear motivating cases.

---

_Closed by @epage on 2025-08-07 20:08_

---

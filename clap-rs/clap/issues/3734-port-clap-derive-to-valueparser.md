---
number: 3734
title: "Port `clap_derive` to `ValueParser`"
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - E-hard
  - A-derive
assignees: []
created_at: 2022-05-18T00:23:22Z
updated_at: 2022-05-25T18:02:52Z
url: https://github.com/clap-rs/clap/issues/3734
synced_at: 2026-01-10T01:27:45Z
---

# Port `clap_derive` to `ValueParser`

---

_Issue opened by @epage on 2022-05-18 00:23_

Adjusting the derive API to #3732 was proving to be difficult because of the current traits take `&ArgMatches` and because `FromArgMatches::update_from_arg_matches` derive for subcommands assumes that `ArgMatches` remains untouched.

Implementing the derive will automatically address
- #3496
- #3199
- #3589

It will also let us deprecate
- [x] Deprecate possible_values in favor of value_parser
- [x] Deprecate allow_invalid_utf8 in favor of ValueParser
- [x] Deprecate arg value validators in favor of ValueParser
- [x] Deprecate forbid_empty_values
- [x] Deprecate value_* in favor of get_*

---

_Label `C-enhancement` added by @epage on 2022-05-18 00:23_

---

_Label `E-hard` added by @epage on 2022-05-18 00:23_

---

_Label `A-derive` added by @epage on 2022-05-18 00:23_

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-18 00:25_

---

_Added to milestone `3.x` by @epage on 2022-05-18 00:25_

---

_Closed by @epage on 2022-05-23 15:33_

---

_Referenced in [clap-rs/clap#3747](../../clap-rs/clap/pulls/3747.md) on 2022-05-24 21:25_

---

_Referenced in [clap-rs/clap#3753](../../clap-rs/clap/pulls/3753.md) on 2022-05-25 18:02_

---

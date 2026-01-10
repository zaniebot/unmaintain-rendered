---
number: 80
title: Add support for building arguments from yaml
type: issue
state: closed
author: kbknapp
labels:
  - A-builder
assignees: []
created_at: 2015-04-28T22:45:40Z
updated_at: 2015-09-01T04:04:03Z
url: https://github.com/clap-rs/clap/issues/80
synced_at: 2026-01-10T01:26:22Z
---

# Add support for building arguments from yaml

---

_Issue opened by @kbknapp on 2015-04-28 22:45_

Adds a `feature` and an additional dep, which is **not** included by default.

To be released this should support the following methods of `App`, `Arg`, and `ArgGroup`
### App
- [x] `App::new` (i.e. setting the name)
- [x] `App::version`
- [x] `App::author`
- [x] `App::bin_name`
- [x] `App::about`
- [x] `App::after_help`
- [x] `App::usage`
- [x] `App::help`
- [x] `App::help_short`
- [x] `App::version_short`
- [x] `App::setting(s)`
- [x] `App::arg(s)`
- [x] `App::arg_group(s)`
- [x] `App::subcommand(s)`

The following will **not** be implemented, yet.
- `App::arg(s)_from_usage`
- All methods deprecated by `App::setting(s)`
### Arg
- [x] `Arg::with_name`
- [x] `Arg::short`
- [x] `Arg::long`
- [x] `Arg::help`
- [x] `Arg::required`
- [x] `Arg::takes_value`
- [x] `Arg::index`
- [x] `Arg::multiple`
- [x] `Arg::global`
- [x] `Arg::empty_values`
- [x] `Arg::group`
- [x] `Arg::number_of_values`
- [x] `Arg::max_values`
- [x] `Arg::min_values`
- [x] `Arg::value_name`
- [x] `Arg::value_names`
- [x] `Arg::conflicts_with`
- [x] `Arg::requires`
- [x] `Arg::mutually_overrides_with`
- [x] `Arg::possible_values`

The following will **not** be implemented, yet.
- `Arg::validator`
- `Arg::*_all` (These because the functionality is implemented in the single method)
- `Arg::from_usage`
### ArgGroup
- [x] `ArgGroup::with_name` (i.e. setting the name)
- [x] `ArgGroup::add`
- [x] `ArgGroup::required`
- [x] `ArgGroup::requires`
- [x] `ArgGroup::conflicts_with`

The following will **not** be implemented, yet.
- `ArgGroup::*_all` (These because the functionality is implemented in the single method)


---

_Label `feature request` added by @kbknapp on 2015-04-28 22:45_

---

_Renamed from "Add support for building arguments from yaml or toml" to "Add support for building arguments from yaml" by @kbknapp on 2015-04-29 01:23_

---

_Referenced in [clap-rs/clap#81](../../clap-rs/clap/issues/81.md) on 2015-04-29 01:25_

---

_Label `optional dep` added by @kbknapp on 2015-05-04 04:15_

---

_Comment by @kbknapp on 2015-05-11 17:43_

Postponed, waiting for a good Rust yaml lib to take shape. Potentially [this one](https://github.com/kimhyunkang/libyaml-rust)


---

_Label `postponed` added by @kbknapp on 2015-05-11 17:44_

---

_Comment by @Byron on 2015-05-17 12:34_

Maybe [this project](https://github.com/serde-rs/serde/pull/73) could also add suitable yaml support - stay tuned ;).


---

_Comment by @kbknapp on 2015-05-17 13:15_

Awesome! Thanks for the heads up!


---

_Comment by @Byron on 2015-06-02 04:57_

You might be able to use the brand-new [rust-yaml](https://users.rust-lang.org/t/feedback-request-yaml-rust-the-missing-yaml-1-2-parser-for-pure-rust/1659) right away ! It is very light-weight and comes with no dependencies. The only 'disadvantage' is that serialization/deserialization to/from structs is not yet supported - that should be provided at some point in future by the already linked _serde-yaml_, but seems to be planned by _yaml-rust_ based on `rustc-serialize`.


---

_Comment by @kbknapp on 2015-06-02 11:25_

Nice! I'll take a closer look here shortly. :+1


---

_Added to milestone `v1.2` by @kbknapp on 2015-07-06 01:24_

---

_Label `C: args` added by @kbknapp on 2015-07-15 04:19_

---

_Label `C: app` added by @kbknapp on 2015-07-15 04:19_

---

_Label `D: intermediate` added by @kbknapp on 2015-07-15 04:19_

---

_Label `P3: want to have` added by @kbknapp on 2015-07-15 04:19_

---

_Label `W: postponed` removed by @kbknapp on 2015-07-27 12:25_

---

_Removed from milestone `v1.2` by @kbknapp on 2015-08-20 03:09_

---

_Added to milestone `1.3` by @kbknapp on 2015-08-30 16:49_

---

_Assigned to @kbknapp by @kbknapp on 2015-08-31 02:51_

---

_Closed by @kbknapp on 2015-09-01 04:04_

---

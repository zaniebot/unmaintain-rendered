```yaml
number: 1801
title: required_unless, required_unless_one, and required_unless_all for ArgGroup
type: issue
state: open
author: AuroransSolis
labels:
  - C-enhancement
  - A-validators
  - S-waiting-on-mentor
assignees: []
created_at: 2020-04-09T10:05:06Z
updated_at: 2021-12-09T22:14:20Z
url: https://github.com/clap-rs/clap/issues/1801
synced_at: 2026-01-12T16:14:11Z
```

# required_unless, required_unless_one, and required_unless_all for ArgGroup

---

_@AuroransSolis_

### Describe your use case

The issue this aims to solve is the lack of `required_unless`, `required_unless_one`, and `required_unless_all` for `ArgGroup`. For example, one might have the following:
```rust
let app = App::new("example")
    .arg(Arg::with_name("foo-user")
        .takes_value(true)
        .long("foo-user"))
    .arg(Arg::with_name("foo-user-file")
        .takes_value(true)
        .long("foo-user-file"))
    .group(ArgGroup::with_name("foo-username")
        .args(&["foo-user", "foo-user-file"])
        .multiple(false)
        .requires("foo-password"))
    .arg(Arg::with_name("foo-pass")
        .takes_value(true)
        .long("foo-pass"))
    .arg(Arg::with_name("foo-pass-file")
        .takes_value(true)
        .long("foo-pass-file"))
    .group(ArgGroup::with_name("foo-password")
        .args(&["foo-pass", "foo-pass-file"])
        .multiple(false))
    .group(ArgGroup::with_name("foo-creds")
        .args(&["foo-username", "foo-password"])
        .requires_all(&["foo-username", "foo-password"])
        .multiple(true)
        .required_unless("bar-creds"))
    .arg(Arg::with_name("bar-user")
        .takes_value(true)
        .requires("bar-password")
        .long("bar-user"))
    .arg(Arg::with_name("bar-user-file")
        .takes_value(true)
        .requires("bar-password")
        .long("bar-user-file"))
    .group(ArgGroup::with_name("bar-username")
        .args(&["bar-user", "bar-user-file"])
        .multiple(false))
    .arg(Arg::with_name("bar-pass")
        .takes_value(true)
        .long("bar-pass"))
    .arg(Arg::with_name("bar-pass-file")
        .takes_value(true)
        .long("bar-pass-file"))
    .group(ArgGroup::with_name("bar-password")
        .args(&["bar-pass", "bar-pass-file"])
        .multiple(false))
    .group(ArgGroup::with_name("bar-creds")
        .args(&["bar-username", "bar-password"])
        .multiple(true))
    .arg(Arg::with_name("bar-fallback-to-foo")
        .takes_value(false)
        .requires_all(&["foo-creds", "bar-creds"]));
```
One might imagine a different situation where `required_unless_one` or `required_unless_all` might be of use as well, but the only short example I could come up with is one demonstrating the use of `required_unless`. This reduces the number of lines required to express certain requirements by moving a `required_unless`, `required_unless_one`, or `required_unless_all` method from each argument in the group to a single one on the group itself. This also makes requirements for group arguments more clear and maintainable, as they're located in a single spot instead of on each group argument.

### Alternatives

It may be possible in some situations to use `required_unless`, `required_unless_one`, or `required_unless_all` on the arguments in the group one might otherwise apply to the group itself, but this would be at least one line per argument, possibly more if the slices for `required_unless_one` or `required_unless_all` are particularly long.


---

_Label `T: new feature` added by @AuroransSolis on 2020-04-09 10:05_

---

_Label `C: arg groups` added by @pksunkara on 2020-04-09 10:15_

---

_Label `D: easy` added by @pksunkara on 2020-04-09 10:15_

---

_Label `P3: want to have` added by @pksunkara on 2020-04-09 10:15_

---

_Label `Z: good first issue` added by @pksunkara on 2020-04-09 10:15_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 10:15_

---

_Comment by @j-delaney on 2020-04-10 12:18_

I'm happy to take a shot at this if that's okay!

---

_Comment by @Dylan-DPC-zz on 2020-04-10 12:18_

Go ahead. Thanks 

---

_Referenced in [clap-rs/clap#1827](../../clap-rs/clap/pulls/1827.md) on 2020-04-15 21:38_

---

_Closed by @pksunkara on 2020-05-20 14:13_

---

_Reopened by @pksunkara on 2020-05-20 14:13_

---

_Comment by @pksunkara on 2020-05-20 14:14_

Better way to fix this would be implementing the following way

> factoring out most of Arg and ArgGroup into new ArgBase struct to avoid code duplication

---

_Referenced in [epage/clapng#152](../../epage/clapng/issues/152.md) on 2021-12-06 20:12_

---

_Label `C: arg groups` removed by @epage on 2021-12-08 19:57_

---

_Label `A-builder` added by @epage on 2021-12-08 19:57_

---

_Label `C: validators` added by @epage on 2021-12-08 19:57_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:13_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:13_

---

_Label `A-builder` removed by @epage on 2021-12-09 22:14_

---

_Label `D: easy` removed by @epage on 2021-12-09 22:14_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 22:14_

---

_Label `E-easy` removed by @epage on 2021-12-09 22:14_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 22:14_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 22:14_

---

_Referenced in [clap-rs/clap#6030](../../clap-rs/clap/issues/6030.md) on 2025-06-10 15:44_

---

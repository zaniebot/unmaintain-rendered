```yaml
number: 354
title: "Allow passing &Arg structs"
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-builder
  - E-medium
assignees: []
created_at: 2015-11-30T12:06:57Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/354
synced_at: 2026-01-12T16:14:09Z
```

# Allow passing &Arg structs

---

_@kbknapp_

Currently only owned `Arg`s are passed to `App`, but I don't believe this required, just the way it was done.

It would be far better, and more idiomatic if one could pass in _either_ an owned or borrowed `Arg` (perhaps with ~~`Cow`~~ `ToOwned`?).

Caveat, this **must** be done in a backwards compat way, if it can't be done and maintain backwards compat, it'll have to wait 2.x


---

_Label `T: enhancement` added by @kbknapp on 2015-11-30 12:06_

---

_Label `C: app` added by @kbknapp on 2015-11-30 12:06_

---

_Label `P3: want to have` added by @kbknapp on 2015-11-30 12:06_

---

_Label `W: 1.x` added by @kbknapp on 2015-11-30 12:06_

---

_Label `M: mentored` added by @kbknapp on 2015-11-30 12:06_

---

_Referenced in [clap-rs/clap#361](../../clap-rs/clap/issues/361.md) on 2015-12-11 02:13_

---

_Comment by @sru on 2016-01-13 07:40_

@kbknapp I think this can be done with `Borrow` ([doc](http://doc.rust-lang.org/nightly/std/borrow/trait.Borrow.html)) because it is implemented for both T and &T.

This should be easy enough because the `add_arg` function only needs owned `Arg` when pushing it into the `global_args`at [here](https://github.com/kbknapp/clap-rs/blob/f0a0e4df50a024a89571c3b56da59816583208db/src/app/mod.rs#L800).


---

_Added to milestone `v2.0` by @kbknapp on 2016-01-23 15:49_

---

_Comment by @kbknapp on 2016-01-25 10:15_

This is closed with 826bc50 using the `Borrow` trait just as @sru said ;)


---

_Closed by @kbknapp on 2016-01-25 10:15_

---

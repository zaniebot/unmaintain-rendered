---
number: 1118
title: Support case insensitve values
type: issue
state: closed
author: kbknapp
labels: []
assignees: []
created_at: 2017-11-28T01:28:09Z
updated_at: 2018-08-02T03:30:15Z
url: https://github.com/clap-rs/clap/issues/1118
synced_at: 2026-01-10T01:26:43Z
---

# Support case insensitve values

---

_Issue opened by @kbknapp on 2017-11-28 01:28_

via `Arg::case_insensitive(bool)` for use with  `Arg::possible_values`

If possible value is `foobar` and the user passes `FoObAR` the value saved will be `foobar` (or whatever the actual possible value is).

This is borderline required for `#[derive(ArgEnum)] enum Foo { FooBar }` due to rust naming rules and not wanting to stray too far outside the norm.d

---

_Label `C: args` added by @kbknapp on 2017-11-28 01:28_

---

_Label `D: intermediate` added by @kbknapp on 2017-11-28 01:28_

---

_Label `P2: need to have` added by @kbknapp on 2017-11-28 01:28_

---

_Label `T: new feature` added by @kbknapp on 2017-11-28 01:28_

---

_Label `W: 2.x` added by @kbknapp on 2017-11-28 01:28_

---

_Closed by @kbknapp on 2017-11-28 15:35_

---

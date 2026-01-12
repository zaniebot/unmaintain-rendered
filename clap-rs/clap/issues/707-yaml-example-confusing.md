```yaml
number: 707
title: yaml example confusing
type: issue
state: closed
author: mvaude
labels:
  - C-bug
assignees: []
created_at: 2016-10-26T13:35:11Z
updated_at: 2018-08-02T03:29:55Z
url: https://github.com/clap-rs/clap/issues/707
synced_at: 2026-01-12T16:14:09Z
```

# yaml example confusing

---

_@mvaude_

I am reading the examples to see how I will use this crate to create some rust CLI binaries.
In the `17_yaml.rs` , it is matching the possible values from the attribute `mode` with `fast` and `slow`.
When I check the `17_yaml.yml`, `mode` possible values are defined as: `vi` or `emacs`.

I just wanted to know if it was a bug in the example or a misunderstanding from me.

Cheers


---

_Comment by @kbknapp on 2016-10-27 04:10_

I'm on mobile right now but this may be a typo in the examples! Thanks for pointing it out! I'll post back here once I can check it on my computer.


---

_Label `T: bug` added by @kbknapp on 2016-10-27 04:13_

---

_Label `P1: urgent` added by @kbknapp on 2016-10-27 04:13_

---

_Label `D: easy` added by @kbknapp on 2016-10-27 04:13_

---

_Label `C: examples` added by @kbknapp on 2016-10-27 04:13_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-27 04:13_

---

_Comment by @mvaude on 2016-10-27 10:26_

Thanks for the update.

I already prepared a working fix if you want to test: https://github.com/myCrates/clap-rs/commit/7d1dbc4c4e77deaad32c3177c9b8dee20206abed.

Let me know if you want a PR to integrate it.


---

_Comment by @kbknapp on 2016-10-27 11:39_

Absolutely! Thanks :+1:


---

_Referenced in [clap-rs/clap#710](../../clap-rs/clap/pulls/710.md) on 2016-10-27 13:14_

---

_Closed by @homu on 2016-10-28 01:55_

---

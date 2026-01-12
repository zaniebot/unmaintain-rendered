```yaml
number: 179
title: Confusing error message
type: issue
state: closed
author: Vinatorul
labels:
  - C-bug
assignees: []
created_at: 2015-08-20T18:20:26Z
updated_at: 2018-08-02T03:29:41Z
url: https://github.com/clap-rs/clap/issues/179
synced_at: 2026-01-12T16:14:08Z
```

# Confusing error message

---

_@Vinatorul_

I found that following code

```
let a = App::new("test")
            .arg(Arg::from_usage("-f1, --flag 'some flag'"));
```

leads to

> panicked at 'called `Option::unwrap()` on a `None` value', ../src/libcore/option.rs:362

I think if happened because of misspell if short argument name.


---

_Comment by @kbknapp on 2015-08-20 21:22_

Hmm...yeah that's why it happened, but I'm not sure _exactly_ where it happened (either in `Arg::from_usage()` or in the usage parser itself. Maybe we should go through and change some of the `unwrap()`s to `expect("message")`s


---

_Label `T: bug` added by @Vinatorul on 2015-08-21 19:44_

---

_Label `C: args` added by @Vinatorul on 2015-08-21 19:44_

---

_Label `D: easy` added by @Vinatorul on 2015-08-21 19:44_

---

_Label `P3: want to have` added by @Vinatorul on 2015-08-21 19:45_

---

_Referenced in [clap-rs/clap#183](../../clap-rs/clap/pulls/183.md) on 2015-08-22 09:44_

---

_Closed by @Vinatorul on 2015-08-22 14:28_

---

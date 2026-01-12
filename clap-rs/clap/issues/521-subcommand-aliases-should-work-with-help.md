```yaml
number: 521
title: "Subcommand aliases should work with \"help\""
type: issue
state: closed
author: joshtriplett
labels:
  - C-bug
assignees: []
created_at: 2016-06-06T02:22:09Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/521
synced_at: 2026-01-12T16:14:09Z
```

# Subcommand aliases should work with "help"

---

_@joshtriplett_

If I have a subcommand `foo`, with an alias `bar`, then `cmd bar` will work, but `cmd help bar` will say `error: The subcommand 'bar' wasn't recognized`.

Ideally, `cmd help bar` should show the help for `foo`, with a preface that `bar` is an alias for `foo`.


---

_Label `T: bug` added by @kbknapp on 2016-06-06 23:09_

---

_Label `C: subcommands` added by @kbknapp on 2016-06-06 23:09_

---

_Label `D: easy` added by @kbknapp on 2016-06-06 23:09_

---

_Label `P3: want to have` added by @kbknapp on 2016-06-06 23:09_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-06 23:09_

---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-08 02:04_

---

_Comment by @kbknapp on 2016-06-10 01:58_

This is fixed with #525 


---

_Closed by @homu on 2016-06-10 10:38_

---

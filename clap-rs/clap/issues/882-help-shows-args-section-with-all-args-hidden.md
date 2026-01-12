```yaml
number: 882
title: HELP shows ARGS section with all args hidden
type: issue
state: closed
author: nagisa
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2017-03-01T16:46:06Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/882
synced_at: 2026-01-12T16:14:10Z
```

# HELP shows ARGS section with all args hidden

---

_@nagisa_

app.arg(Arg::with_name("dummy").possible_value("some val").required(false).hidden(true));

Results in

```
USAGE:
    app [dummy] [SUBCOMMAND]

FLAGS:
    ...

ARGS:
   { EMPTY }

SUBCOMMANDS:
   ...
``` 

Additional comments: 

If `possible_value` is used, `USAGE` ought to display the possible value instead of arg name?

### Rust Version

N/A

### Affected Version of clap

2.20.5

---

_Comment by @kbknapp on 2017-03-01 17:10_

Thanks for reporting, this should be an easy fix!

---

_Label `C: help message` added by @kbknapp on 2017-03-01 17:10_

---

_Label `D: easy` added by @kbknapp on 2017-03-01 17:10_

---

_Label `P2: need to have` added by @kbknapp on 2017-03-01 17:10_

---

_Label `T: bug` added by @kbknapp on 2017-03-01 17:10_

---

_Label `W: 2.x` added by @kbknapp on 2017-03-01 17:10_

---

_Referenced in [clap-rs/clap#884](../../clap-rs/clap/pulls/884.md) on 2017-03-03 08:39_

---

_Closed by @homu on 2017-03-11 18:31_

---

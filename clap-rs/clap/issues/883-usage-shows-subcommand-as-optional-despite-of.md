---
number: 883
title: USAGE shows SUBCOMMAND as optional despite of SubcommandRequiredElseHelp option
type: issue
state: closed
author: nagisa
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2017-03-01T16:51:59Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/883
synced_at: 2026-01-10T01:26:37Z
---

# USAGE shows SUBCOMMAND as optional despite of SubcommandRequiredElseHelp option

---

_Issue opened by @nagisa on 2017-03-01 16:51_

### Rust Version

N/A

### Affected Version of clap

* 2.20.5

---

With 

```
app.setting(AppSettings::SubcommandRequiredElseHelp)
```

the USAGE help still shows:

```
USAGE:
    app [dummy] [SUBCOMMAND]
```

It probably shouldn't show the subcommand as optional.

---

_Comment by @kbknapp on 2017-03-01 17:12_

Agreed. Thanks!

---

_Label `C: help message` added by @kbknapp on 2017-03-01 17:12_

---

_Label `C: subcommands` added by @kbknapp on 2017-03-01 17:12_

---

_Label `D: easy` added by @kbknapp on 2017-03-01 17:12_

---

_Label `P2: need to have` added by @kbknapp on 2017-03-01 17:12_

---

_Label `T: bug` added by @kbknapp on 2017-03-01 17:12_

---

_Comment by @nagisa on 2017-03-01 17:25_

IIRC this may also happen with SubcommandRequired option as well.

---

_Comment by @kbknapp on 2017-03-02 15:20_

Yeah I think it's also in the same family of issues as #871 I should have some time to sit down and knock it out this family of issues tomorrow.

---

_Closed by @homu on 2017-03-11 18:31_

---

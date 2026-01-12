```yaml
number: 133
title: "Allow consumer to specify \"display help if nothing else\""
type: issue
state: closed
author: kbknapp
labels: []
assignees: []
created_at: 2015-05-25T17:11:07Z
updated_at: 2015-05-26T01:56:41Z
url: https://github.com/clap-rs/clap/issues/133
synced_at: 2026-01-12T16:14:08Z
```

# Allow consumer to specify "display help if nothing else"

---

_@kbknapp_

Will probably be implemented as two methods on `App`; `.help_on_no_args()` and `.help_on_no_subcommand()`


---

_Label `feature request` added by @kbknapp on 2015-05-25 17:11_

---

_Assigned to @kbknapp by @kbknapp on 2015-05-25 17:11_

---

_Closed by @kbknapp on 2015-05-26 01:53_

---

_Comment by @kbknapp on 2015-05-26 01:56_

This is complete, but the names of the methods changed from the proposed to `.arg_required_else_help()` and `.subcommand_required_else_help()`


---

_Referenced in [clap-rs/clap#2513](../../clap-rs/clap/issues/2513.md) on 2022-01-11 17:45_

---

_Referenced in [clap-rs/clap#3280](../../clap-rs/clap/issues/3280.md) on 2022-01-11 18:03_

---

```yaml
number: 50
title: Add all preceding positionals to required list when one is required
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - C-enhancement
assignees: []
created_at: 2015-04-02T03:40:36Z
updated_at: 2015-04-03T02:22:48Z
url: https://github.com/clap-rs/clap/issues/50
synced_at: 2026-01-12T16:14:08Z
```

# Add all preceding positionals to required list when one is required

---

_@kbknapp_

Currently if you have several positional arguments, and one of the latter ones is required, the preceding ones are not added to the required list, which can make for a confusing error message (i.e. you're putting all the required arguments in according to the usage statement, but still getting "one or more required arguments not supplied")


---

_Label `bug` added by @kbknapp on 2015-04-02 03:40_

---

_Label `enhancement` added by @kbknapp on 2015-04-02 03:40_

---

_Assigned to @kbknapp by @kbknapp on 2015-04-02 03:40_

---

_Referenced in [clap-rs/clap#57](../../clap-rs/clap/pulls/57.md) on 2015-04-03 02:21_

---

_Closed by @kbknapp on 2015-04-03 02:22_

---

```yaml
number: 51
title: Improve error message when argument is mutually excluded
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - C-enhancement
assignees: []
created_at: 2015-04-02T03:42:51Z
updated_at: 2015-04-03T02:42:44Z
url: https://github.com/clap-rs/clap/issues/51
synced_at: 2026-01-12T16:14:08Z
```

# Improve error message when argument is mutually excluded

---

_@kbknapp_

Currently if an argument is mutually excluded by another argument and appears _before_ the argument that declared the exclusion, the error message only states "a required argument wasn't supplied" but it should state that the specific argument is excluded by another (which it correctly does when the excluded argument appears _after_ the one declaring the exclusion).


---

_Label `bug` added by @kbknapp on 2015-04-02 03:42_

---

_Label `enhancement` added by @kbknapp on 2015-04-02 03:42_

---

_Assigned to @kbknapp by @kbknapp on 2015-04-02 03:42_

---

_Closed by @kbknapp on 2015-04-03 02:42_

---

_Referenced in [clap-rs/clap#4510](../../clap-rs/clap/issues/4510.md) on 2022-11-26 07:58_

---

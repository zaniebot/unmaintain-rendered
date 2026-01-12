```yaml
number: 185
title: Redesign internal get_matches_with method
type: issue
state: closed
author: Vinatorul
labels:
  - C-enhancement
  - A-builder
assignees: []
created_at: 2015-08-22T15:13:02Z
updated_at: 2018-08-02T03:29:42Z
url: https://github.com/clap-rs/clap/issues/185
synced_at: 2026-01-12T16:14:08Z
```

# Redesign internal get_matches_with method

---

_@Vinatorul_

I think we need some refactorings here. For now this method is huge (about 500 lines), hard to understand and work with.
Also it will be much more easier to fix bugs like #184.
I think I`ll do it step by step splitting it into lesser methods. 
If anyone have suggestions - let me know


---

_Label `T: enhancement` added by @Vinatorul on 2015-08-22 15:13_

---

_Label `P4: nice to have` added by @Vinatorul on 2015-08-22 15:13_

---

_Label `C: args` added by @Vinatorul on 2015-08-22 15:13_

---

_Label `C: app` added by @Vinatorul on 2015-08-22 15:13_

---

_Label `D: intermediate` added by @Vinatorul on 2015-08-22 15:13_

---

_Label `E: tedious` added by @Vinatorul on 2015-08-22 15:13_

---

_Referenced in [clap-rs/clap#186](../../clap-rs/clap/pulls/186.md) on 2015-08-22 17:08_

---

_Renamed from "Redesign get_matches_with method" to "Redesign internal get_matches_with method" by @kbknapp on 2015-09-10 18:50_

---

_Closed by @kbknapp on 2016-03-10 21:04_

---

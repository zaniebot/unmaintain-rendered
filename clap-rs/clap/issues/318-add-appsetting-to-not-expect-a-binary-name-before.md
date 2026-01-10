---
number: 318
title: Add AppSetting to NOT expect a binary  name before args
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
assignees: []
created_at: 2015-10-13T18:35:01Z
updated_at: 2015-10-29T07:02:08Z
url: https://github.com/clap-rs/clap/issues/318
synced_at: 2026-01-10T01:26:27Z
---

# Add AppSetting to NOT expect a binary  name before args

---

_Issue opened by @kbknapp on 2015-10-13 18:35_

There are times when you may want to match over and over against valid args, but the binary name may not precede the args (think daemons, interactive CLIs, etc.)

The workaround without this setting is to insert a fake binary name prior to arg string, but this requires needless allocation, etc.


---

_Label `T: new feature` added by @kbknapp on 2015-10-13 18:35_

---

_Label `D: easy` added by @kbknapp on 2015-10-13 18:35_

---

_Label `P3: want to have` added by @kbknapp on 2015-10-13 18:35_

---

_Label `C: parsing` added by @kbknapp on 2015-10-13 18:35_

---

_Label `W: 1.x` added by @kbknapp on 2015-10-13 18:35_

---

_Added to milestone `1.4.6` by @kbknapp on 2015-10-13 18:35_

---

_Assigned to @kbknapp by @kbknapp on 2015-10-28 13:59_

---

_Comment by @kbknapp on 2015-10-28 18:15_

#327 adds this


---

_Closed by @kbknapp on 2015-10-29 07:02_

---

---
number: 205
title: "Change ArgGroup::add_all from accepting a Vec<&str> to &[&str]"
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - M-breaking-change
assignees: []
created_at: 2015-08-30T16:50:15Z
updated_at: 2015-09-01T04:06:26Z
url: https://github.com/clap-rs/clap/issues/205
synced_at: 2026-01-10T01:26:25Z
---

# Change ArgGroup::add_all from accepting a Vec<&str> to &[&str]

---

_Issue opened by @kbknapp on 2015-08-30 16:50_

Using a `Vec<&str>` is a legacy issue that never got changed, so it's somewhat of a legacy bug. Changing to a `&[&str]` will be an extremely minor "breaking" change, and I believe I've only come across a single repo which it will affect. 

The fix will be to change code from `add_all(vec!["arg1", "arg2"])` -> `add_all(&["arg1", "arg2"])`

Even so, I want to wait until 1.3 so as not to mess with people's SemVer `Cargo.toml` entries.


---

_Label `T: bug` added by @kbknapp on 2015-08-30 16:50_

---

_Label `E: breaking change` added by @kbknapp on 2015-08-30 16:50_

---

_Label `C: args` added by @kbknapp on 2015-08-30 16:50_

---

_Label `D: easy` added by @kbknapp on 2015-08-30 16:50_

---

_Label `P3: want to have` added by @kbknapp on 2015-08-30 16:50_

---

_Added to milestone `1.3` by @kbknapp on 2015-08-30 16:50_

---

_Comment by @Vinatorul on 2015-08-30 16:53_

I can do it in my fork and we'll merge when 1.3 will be ready to release


---

_Comment by @kbknapp on 2015-08-30 16:55_

:+1: 

You'll need to change a few lines in the examples and tests as well which were using the older style.


---

_Assigned to @Vinatorul by @kbknapp on 2015-08-30 16:55_

---

_Referenced in [clap-rs/clap#206](../../clap-rs/clap/pulls/206.md) on 2015-08-30 17:23_

---

_Closed by @kbknapp on 2015-09-01 04:06_

---

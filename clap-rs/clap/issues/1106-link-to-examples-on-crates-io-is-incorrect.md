---
number: 1106
title: Link to examples on crates.io is incorrect
type: issue
state: closed
author: focusaurus
labels:
  - C-enhancement
  - A-docs
  - E-easy
assignees: []
created_at: 2017-11-13T14:39:03Z
updated_at: 2018-08-02T03:30:14Z
url: https://github.com/clap-rs/clap/issues/1106
synced_at: 2026-01-10T01:26:43Z
---

# Link to examples on crates.io is incorrect

---

_Issue opened by @focusaurus on 2017-11-13 14:39_

### Affected Version of clap
* 2.27.1

### Expected Behavior Summary
Viewing docs on [crates.io/clap](https://crates.io/crates/clap), clicking hyperlinks to the git repo, specifically to the "examples/" directory takes me to the correct URL.

### Actual Behavior Summary

Link is a broken 404 (due to the `.git` in the URL path).

### How to Fix

I think if we change the Cargo.toml `repository` link from `repository = "https://github.com/kbknapp/clap-rs.git"` to `repository = "https://github.com/kbknapp/clap-rs"` that will fix it.

----
CC @kivikakk who contributed some of the relevant crates.io rendering code to consider whether it makes sense for crates.io to try to handle both situations directly.

---

_Comment by @kbknapp on 2017-11-13 19:45_

Great catch thanks!

---

_Label `C: docs` added by @kbknapp on 2017-11-13 19:46_

---

_Label `D: easy` added by @kbknapp on 2017-11-13 19:46_

---

_Label `good first issue` added by @kbknapp on 2017-11-13 19:46_

---

_Label `P4: nice to have` added by @kbknapp on 2017-11-13 19:46_

---

_Label `T: enhancement` added by @kbknapp on 2017-11-13 19:46_

---

_Comment by @kbknapp on 2017-11-13 19:47_

I'd venture that I'm not the only one affected by this, as it's anyone who uses the `.git` extension *and* relative links in their readme.

---

_Referenced in [clap-rs/clap#1108](../../clap-rs/clap/pulls/1108.md) on 2017-11-13 21:04_

---

_Closed by @kbknapp on 2017-11-13 22:09_

---

_Comment by @kivikakk on 2017-11-14 00:03_

@kbknapp I'd venture you're right. I'll think about a proper solution for this on crates.io itself.

@focusaurus thank you for the CC! :heart:

---

_Referenced in [rust-lang/crates.io#1170](../../rust-lang/crates.io/pulls/1170.md) on 2017-11-14 02:05_

---

---
number: 22
title: Move argument assertions into app.rs
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
assignees: []
created_at: 2015-03-19T12:19:55Z
updated_at: 2018-08-02T03:29:36Z
url: https://github.com/clap-rs/clap/issues/22
synced_at: 2026-01-07T13:12:19-06:00
---

# Move argument assertions into app.rs

---

_Issue opened by @kbknapp on 2015-03-19 12:19_

Argument assertions to determine if incompatible options have been used are currently evaluated inside arg.rs within the individual method. This allows for certain assertions to pass, that shouldn't, depending on the order in which argument options are listed. 

Although assertions should normally be evaluated as soon as possible, all options must first be present. Thus assertions need to be moved to app.rs inside the `arg()` method. 


---

_Label `bug` added by @kbknapp on 2015-03-19 12:19_

---

_Closed by @kbknapp on 2015-03-21 14:24_

---

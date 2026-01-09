---
number: 69
title: "Add Arg::from_usage(&str) as a shorthand builder"
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
assignees: []
created_at: 2015-04-12T14:53:06Z
updated_at: 2018-08-02T03:29:38Z
url: https://github.com/clap-rs/clap/issues/69
synced_at: 2026-01-07T13:12:19-06:00
---

# Add Arg::from_usage(&str) as a shorthand builder

---

_Issue opened by @kbknapp on 2015-04-12 14:53_

Currently creating simple arguments (those without relational rules) is just as verbose as more complex configurations. Adding something like a `Arg::from_usage()` would greatly simplify building simple arguments.

Example:

```
Arg::from_usage("[name] -f --flag 'a flag used for something'");
// Or a required option that allows multiple occurrences
Arg::from_usage("--some-opt <name>... 'An option that takes multiple values and is required'");
```


---

_Label `enhancement` added by @kbknapp on 2015-04-12 14:53_

---

_Label `feature request` added by @kbknapp on 2015-04-12 14:53_

---

_Comment by @kbknapp on 2015-04-13 18:22_

Closed with 24dcc13


---

_Closed by @kbknapp on 2015-04-13 18:22_

---

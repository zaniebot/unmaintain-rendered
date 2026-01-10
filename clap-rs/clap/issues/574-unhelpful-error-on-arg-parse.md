---
number: 574
title: Unhelpful error on Arg parse
type: issue
state: closed
author: mbStavola
labels:
  - C-enhancement
assignees: []
created_at: 2016-07-07T20:48:09Z
updated_at: 2016-08-28T03:42:32Z
url: https://github.com/clap-rs/clap/issues/574
synced_at: 2026-01-10T01:26:32Z
---

# Unhelpful error on Arg parse

---

_Issue opened by @mbStavola on 2016-07-07 20:48_

For context, this is what was being read in `from_yaml()`:

``` yaml
- es:
  short: e
  help: Sets the ES tag on the output files.
  default_value: 001
  takes_value: true
```

When running my program I got the very confusing:
`thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', ../src/libcore/option.rs:325`

Looking at the backtrace, I was able to narrow it down to a `Arg::from_yaml()` not parsing correctly. But no idea where or why!

The fix was easy: simply wrap 001 in quotes to make it a string. However, it was a bit annoying to figure it out. 

It would be nice to see the context of the `panic!()` in these cases. For example, what arg setting failed.


---

_Comment by @kbknapp on 2016-07-23 17:22_

Thanks for filing this! I agree, I think adding an `expect` instead of `unwrap` will fix this. Once I get through the existing PRs I'll fix this one as well.


---

_Label `T: enhancement` added by @kbknapp on 2016-07-23 17:23_

---

_Label `P4: nice to have` added by @kbknapp on 2016-07-23 17:23_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-23 17:23_

---

_Label `C: yaml parser` added by @kbknapp on 2016-07-23 17:23_

---

_Assigned to @kbknapp by @kbknapp on 2016-07-23 17:23_

---

_Added to milestone `2.10.5` by @kbknapp on 2016-08-26 15:48_

---

_Closed by @kbknapp on 2016-08-28 03:42_

---

---
number: 1260
title: "Confusing error reason when option has no \"long\" name"
type: issue
state: closed
author: vn971
labels: []
assignees: []
created_at: 2018-04-23T16:44:31Z
updated_at: 2018-08-02T03:30:22Z
url: https://github.com/clap-rs/clap/issues/1260
synced_at: 2026-01-07T13:12:19-06:00
---

# Confusing error reason when option has no "long" name

---

_Issue opened by @vn971 on 2018-04-23 16:44_

### Rust Version

* rustc 1.25.0 (84203cac6 2018-03-25)

### Affected Version of clap

*  2.31.2

### Steps to Reproduce the issue

* define an arg like this: `.arg(clap::Arg::with_name("--test"))`
* start a clap program with  ./app_name --test

### Actual Behavior Summary

```
error: Found argument '--test' which wasn't expected, or isn't valid in this context

USAGE:
    wwrap [--test]

For more information try --help
```
As you see, the logging is kinda self-contradictory. "We don't accept --test because we only accept --test".

### Expected Behavior Summary

runtime error at parse time, or any other solution giving correct error reasons

---

_Comment by @vn971 on 2018-04-23 18:12_

UPD: actually the error reason seems to be different. I didn't provide a "long" name for an option, which is why it didn't work. The "Actual Behavior" above seems to be misleading anyway, IMO, so I'm leaving the issue open.

---

_Renamed from "Double dash argument name" to "Confusing error reason when option has no "long" name" by @vn971 on 2018-04-23 18:13_

---

_Comment by @kbknapp on 2018-06-05 01:34_

It looks like maybe the intent was to use [`Arg::from_usage`](https://docs.rs/clap/2.31.2/clap/struct.Arg.html#method.from_usage) instead of `with_name`. I agree it's a slightly confusing message, but I can't actually think of a better way to word it. Perhaps a debug lint if an arg name starts with a single or double hyphen?

I'm going to close this for now as it's working as intended, and a lint against this would need a way to turn off the lint which is more complicated than I think this issue warrants.

---

_Closed by @kbknapp on 2018-06-05 01:34_

---

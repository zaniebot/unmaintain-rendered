---
number: 73
title: Add support for automatic numeric ranges
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
assignees: []
created_at: 2015-04-14T01:17:25Z
updated_at: 2018-08-02T03:29:38Z
url: https://github.com/clap-rs/clap/issues/73
synced_at: 2026-01-10T01:26:22Z
---

# Add support for automatic numeric ranges

---

_Issue opened by @kbknapp on 2015-04-14 01:17_

In addition to #72 something akin to validating numeric ranges goes along nicely with Specific Value Sets. Perhaps a type of `Range`? Thinking out loud...


---

_Label `enhancement` added by @kbknapp on 2015-04-14 01:17_

---

_Label `feature request` added by @kbknapp on 2015-04-14 01:17_

---

_Comment by @kbknapp on 2015-04-14 20:22_

This is going to be postponed until a better solution to #72 is determined. For this issue in particular (Ranges), I'm envisioning going along with an `ArgType`:

``` rust
// Defined in clap:
enum ArgType {
    Int,
    Float,
    Range(i64, i64)  // high,low
}

// user code:
let arg = Arg::with_name("some-range").index(1).type(ArgType::Range(0,5));
```

But we'll see...


---

_Label `enhancement` removed by @kbknapp on 2015-04-25 14:39_

---

_Renamed from "Add support for numeric ranges" to "Add support for automatic numeric ranges" by @kbknapp on 2015-04-25 14:41_

---

_Label `postponed` added by @kbknapp on 2015-04-29 03:36_

---

_Comment by @kbknapp on 2015-04-30 15:17_

Closed for now...


---

_Closed by @kbknapp on 2015-04-30 15:17_

---

_Reopened by @kbknapp on 2015-05-11 17:45_

---

_Label `P4: nice to have` added by @kbknapp on 2015-07-15 04:20_

---

_Label `C: args` added by @kbknapp on 2015-07-15 04:20_

---

_Label `D: intermediate` added by @kbknapp on 2015-07-15 04:20_

---

_Label `C: parsing` added by @kbknapp on 2015-07-15 04:23_

---

_Comment by @kbknapp on 2015-08-17 19:02_

This is somewhat possible now with #174 being merged, but its still mostly manual. Once we add some generic preconceived arg validations this may be closed. 


---

_Comment by @kbknapp on 2017-01-30 04:59_

[validators](https://docs.rs/clap/2.20.0/clap/struct.Arg.html#method.validator) serve this purpose fine. Closing.

---

_Closed by @kbknapp on 2017-01-30 04:59_

---

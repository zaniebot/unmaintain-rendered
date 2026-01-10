---
number: 664
title: args_from_usage() has issues with non-ASCII identifiers
type: issue
state: closed
author: tormol
labels:
  - C-enhancement
assignees: []
created_at: 2016-09-15T21:33:00Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/664
synced_at: 2026-01-10T01:26:33Z
---

# args_from_usage() has issues with non-ASCII identifiers

---

_Issue opened by @tormol on 2016-09-15 21:33_

Non-ASCII help on its own is OK:
`clap::App::new("").args_from_usage("<h>  'with ŭnicode'");` works

If the last character is non-ascii there are slice errors:
`clap::App::new("").args_from_usage("<hæ> 'comment doesn't matter'");`

If both name and help has non-ascii the program hangs:
`clap::App::new("").args_from_usage("<øh>  'with ŭnicode'");`

If only the name is non-ascii the argument is not detected:
`clap::App::new("").args_from_usage("<øh> 'ASCII or empty'").get_matches().value_of("øh").unwrap()`

The first three act the same with options, but this is different:
`clap::App::new("").args_from_usage("--øh  'ASCII'").get_matches();` gives this error:

```
error: Found argument '--øh' which wasn't expected, or isn't valid in this context
    Did you mean --ø?

USAGE:
    clapbug --ø <ASCII'>

For more information try --help
```

Arg::with_name() has none of these issues.


---

_Comment by @kbknapp on 2016-09-16 21:18_

Originally the `from_usage` parser was designed around ASCII only support (where as full `Arg` methods support any valid UTF-8). I agree this is something I need to add, and a feature I'd like to have. I'll add it to the issue tracker, but there a few other things I need to implement before this.

Thanks for taking the time to request and file this! :+1: 


---

_Label `T: enhancement` added by @kbknapp on 2016-09-16 21:18_

---

_Label `C: args` added by @kbknapp on 2016-09-16 21:18_

---

_Label `D: intermediate` added by @kbknapp on 2016-09-16 21:18_

---

_Label `P3: want to have` added by @kbknapp on 2016-09-16 21:18_

---

_Label `W: 2.x` added by @kbknapp on 2016-09-16 21:18_

---

_Comment by @tormol on 2016-09-17 05:35_

This is a niche issue, and easy to work around.
Asserting that the identifiers are ascii to fail in a consistent way should be enough.


---

_Comment by @nabijaczleweli on 2016-09-17 05:40_

Why'd you ever fail when it can actually work though?


---

_Referenced in [clap-rs/clap#670](../../clap-rs/clap/pulls/670.md) on 2016-09-27 23:04_

---

_Comment by @tormol on 2016-09-30 15:34_

If it was difficult to make it work. I did it anyway.


---

_Closed by @homu on 2016-10-04 16:20_

---

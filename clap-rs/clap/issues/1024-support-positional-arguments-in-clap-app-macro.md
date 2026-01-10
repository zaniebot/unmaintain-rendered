---
number: 1024
title: "Support positional arguments in `clap_app!` macro"
type: issue
state: closed
author: droundy
labels: []
assignees: []
created_at: 2017-08-09T11:36:43Z
updated_at: 2021-04-16T04:02:32Z
url: https://github.com/clap-rs/clap/issues/1024
synced_at: 2026-01-10T01:26:41Z
---

# Support positional arguments in `clap_app!` macro

---

_Issue opened by @droundy on 2017-08-09 11:36_

I would like for the `clap_app!` macro to support positional arguments.  If it does support them, then I would love for this to be documented.



---

_Comment by @jaccarmac on 2017-08-21 04:53_

I've been able to use the following pattern to get positional args:

```
clap_app!(app =>
    (@arg first: "first")
    (@arg second: "second"))
```

If they are required as positional args often are `(@arg first: * "first")` will make it so, and can be used like so: `(@arg first: * default_value[default] "first")` to get a default value simultaneously (thus the arg shows up in the command line documentation as if it is required but in fact is not required).

---

_Comment by @kbknapp on 2017-10-04 02:55_

Thanks @jaccarmac :+1: 

I'm going to close this. Feel free to re-open if it needs further discussion.

---

_Closed by @kbknapp on 2017-10-04 02:55_

---

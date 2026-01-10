---
number: 807
title: "Feature request: Arg::requires_any"
type: issue
state: closed
author: saghm
labels: []
assignees: []
created_at: 2017-01-04T06:08:08Z
updated_at: 2018-08-02T03:29:59Z
url: https://github.com/clap-rs/clap/issues/807
synced_at: 2026-01-10T01:26:36Z
---

# Feature request: Arg::requires_any

---

_Issue opened by @saghm on 2017-01-04 06:08_

Hi! First off, sorry for not following the template! The default sections seemed to be geared towards bug reports, and I honestly didn't think that any of them applied to my issue. As for the request...

I'd like to be able to specify that an argument requires any one (or more) of multiple other arguments. Right now, I can do so manually in my application logic, but from looking over the documentation, I don't think there's currently a way to tell clap to do this on its own. Ideally it would look pretty similar to `Args::requires_all`, only it would reject the input only if none of the specified arguments are passed in.

Thanks for taking the time to consider this! I completely understand if the balance of work to implement vs. priority means this won't be implemented for a while.

---

_Comment by @kbknapp on 2017-01-04 14:22_

No worries on the template, it's really just there for bug reports :+1: 

----

This can be accomplished with an `ArgGroup`

For example:

```rust
let app = App::new("foo")
    .arg(Arg::with_name("base")
        .requires("others_group"))
    .arg(Arg::with_name("opt1")
        .long("opt-1")
        .takes_value(true))
    .arg(Arg::with_name("opt2")
        .long("opt-2")
        .takes_value(true))
    .arg(Arg::with_name("opt3")
        .long("opt-3")
        .takes_value(true))
    .group(ArgGroup::with_name("others_group")
        .args(&["opt1", "opt2", "opt3"]));
```

Let me know if that works for you :wink:

---

_Label `T: RFC / question` added by @kbknapp on 2017-01-04 14:22_

---

_Comment by @saghm on 2017-01-04 20:59_

Awesome, this works perfectly. Sorry about not figuring this out on my own!

---

_Closed by @saghm on 2017-01-04 20:59_

---

_Comment by @kbknapp on 2017-01-04 22:43_

No worries, that's what I'm here for! :+1:

---

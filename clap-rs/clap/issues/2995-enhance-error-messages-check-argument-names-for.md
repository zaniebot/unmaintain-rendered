---
number: 2995
title: Enhance error messages, check argument names for correctness
type: issue
state: closed
author: Tastaturtaste
labels: []
assignees: []
created_at: 2021-11-05T16:58:15Z
updated_at: 2021-11-05T17:59:01Z
url: https://github.com/clap-rs/clap/issues/2995
synced_at: 2026-01-10T01:27:30Z
---

# Enhance error messages, check argument names for correctness

---

_Issue opened by @Tastaturtaste on 2021-11-05 16:58_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Describe your use case

Enhance error reporting to better guide the user to the underlying issue and to the corresponding documentation.
The (minimalized) issue triggering this request was me trying to set a argument as follows: 
```
use clap::{App, Arg};

fn main() {
    let args = App::new("prog")
        .arg(Arg::with_name("flag").long("flag").short("ff"))
        .get_matches_from(vec!["prog", "-ff"]);
    println!("{:?}", args);
}
```
As you can see I foolishly tried to set `.short("ff")` which does not work since [only the first non `-` character is used as the short argument](https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.short).
While the docs are quite clear about it, the error message I got is much less useful in my opinion: 
> error: The argument '--flag' was provided more than once, but cannot be used multiple times

My original use case involved more arguments, custom parsing, structopt, etc. and the cause of this issue was very much not obvious to me.

### Describe the solution you'd like

What I would have liked to happen would be a Error or panic! out of the `.short` method, even before matching of the actual arguments happens, indicating that the provided short-name is not allowed. 
While the docs are clear regarding this issue I did not even know to look there, instead thinking my custom parsing or the order of arguments messed some things up. 

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @Tastaturtaste on 2021-11-05 16:58_

---

_Comment by @epage on 2021-11-05 17:06_

> .short("ff")

`clap` v3 changed the type for `short` from `&str` to` char`, so this will now be a compile-time error.

---

_Comment by @Tastaturtaste on 2021-11-05 17:32_

Wow, nice. I should maybe switch to the beta.
Also, that has to be one of the fastest responses I ever got on github, thanks 👍 

---

_Closed by @Tastaturtaste on 2021-11-05 17:32_

---

_Comment by @epage on 2021-11-05 17:59_

Docs are a weak point; we are waiting to see how things shake up first.  You can track how close we are at https://github.com/clap-rs/clap/milestone/76.

---

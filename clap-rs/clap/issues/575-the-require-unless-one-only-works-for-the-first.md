---
number: 575
title: "The `require_unless_one` only works for the first argument name in array"
type: issue
state: closed
author: volks73
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-07-08T19:45:13Z
updated_at: 2016-07-23T22:10:38Z
url: https://github.com/clap-rs/clap/issues/575
synced_at: 2026-01-10T01:26:32Z
---

# The `require_unless_one` only works for the first argument name in array

---

_Issue opened by @volks73 on 2016-07-08 19:45_

Let me start by saying that this library (crate) is amazing. It has been a lot of fun creating small CLI applications with Rust and this library. This is also my first time submitting an issue on Github.

I have been trying to use the `require_unless_one` function for an argument. It appears to only work for the first argument name in the array. From the documentation, this works:

``` rust
let res = App::new("unlessone")
    .arg(Arg::with_name("cfg")
        .required_unless_one(&["dbg", "infile"])
        .takes_value(true)
        .long("config"))
    .arg(Arg::with_name("dbg")
        .long("debug"))
    .arg(Arg::with_name("infile")
        .short("i")
        .takes_value(true))
    .get_matches_from_safe(vec![
        "unlessone", "--debug"
    ]);

    assert!(res.is_ok());
    let m = res.unwrap();
    assert!(m.is_present("dbg"));
    assert!(!m.is_present("cfg"));
```

But the following does not appear to work:

``` rust
let res = App::new("unlessone")
    .arg(Arg::with_name("cfg")
        .required_unless_one(&["dbg", "infile"])
        .takes_value(true)
        .long("config"))
    .arg(Arg::with_name("dbg")
        .long("debug"))
    .arg(Arg::with_name("infile")
        .short("i")
        .takes_value(true))
    .get_matches_from_safe(vec![
        "unlessone", "-i", "file"
    ]);

    assert!(res.is_ok());
    let m = res.unwrap();
    assert!(m.is_present("infile"));
    assert!(!m.is_present("cfg"));
```

Note, I have specified the `infile` argument, which is the second value in the array for the `required_unless_one` function. I have forked the repository and added a test for the second example. It currently fails. I have not had a chance to find the issue in the source code yet, but I thought I could at least submit an issue and a pull request that adds the test.

I am using the latest release version of clap (v2.9.2) on Mac OSX with Rust 1.9.0 stable.


---

_Referenced in [clap-rs/clap#576](../../clap-rs/clap/pulls/576.md) on 2016-07-08 19:50_

---

_Comment by @kbknapp on 2016-07-23 17:26_

Thanks for filing this, and also the PR :wink: I apologize for the time it's taken me to get to this; I've been traveling for my job. I'm going to look through your PR and we should be able to get this fixed quickly. :+1: 


---

_Label `T: bug` added by @kbknapp on 2016-07-23 17:26_

---

_Label `P2: need to have` added by @kbknapp on 2016-07-23 17:26_

---

_Label `D: easy` added by @kbknapp on 2016-07-23 17:26_

---

_Label `C: parsing` added by @kbknapp on 2016-07-23 17:26_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-23 17:26_

---

_Assigned to @kbknapp by @kbknapp on 2016-07-23 17:28_

---

_Comment by @kbknapp on 2016-07-23 21:23_

#595 fixes this


---

_Closed by @homu on 2016-07-23 22:10_

---

---
number: 607
title: wrong erro msgs for arg dependecies.
type: issue
state: closed
author: laishulu
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-07-28T10:19:43Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/607
synced_at: 2026-01-07T13:12:19-06:00
---

# wrong erro msgs for arg dependecies.

---

_Issue opened by @laishulu on 2016-07-28 10:19_

If I have args:

``` yaml
- a:
   required=true
- b:
   required=false
- c:
   required=false
   requires:
     - b
```

and I invoke as `myapp -a -c` then I got the following error msg:

> error: The following required arguments were not provided
>      -a
>      -b

but, `-a` should not be listed in the error msgs because it's already presented in the command args


---

_Comment by @kbknapp on 2016-07-29 01:21_

This would be a bug. Let me reproduce and I should be able to fix it pretty easily. Thanks!


---

_Label `T: bug` added by @kbknapp on 2016-07-29 01:22_

---

_Label `P2: need to have` added by @kbknapp on 2016-07-29 01:22_

---

_Label `C: args` added by @kbknapp on 2016-07-29 01:22_

---

_Label `D: easy` added by @kbknapp on 2016-07-29 01:22_

---

_Label `C: parsing` added by @kbknapp on 2016-07-29 01:22_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-29 01:22_

---

_Comment by @kbknapp on 2016-07-29 01:26_

I'm not able to reproduce. Could you link to the code that defines the arguments?

I tested:

``` rust
fn main() {
    let m = App::new("test")
        .arg(Arg::with_name("a")
            .required(true)
            .takes_value(true)
            .short("a"))
        .arg(Arg::with_name("b")
            .short("b"))
        .arg(Arg::with_name("c")
            .requires("b")
            .short("c"))
        .get_matches();
}
```

And ran with

```
$ test -a val -c
error: The following required arguments were not provided:
    -b

USAGE:
    test -b -c -a <a>

For more information try --help
```


---

_Comment by @laishulu on 2016-07-29 03:44_

The bug seems related with groups

``` rust
fn main() {
    let m = App::new("test")
        .arg(Arg::with_name("a").short("a"))
        .arg(Arg::with_name("b").short("b"))

        .arg(Arg::with_name("c").short("c"))
        .arg(Arg::with_name("d").short("d").requires("c"))

        .group(ArgGroup::with_name("flag")
         .args(&["a", "b"])
         .required(true))
        .get_matches();
}
```

and ran with

```
 $ cargo run -- -d -a                          [11:40:02]
     Running `target/debug/test -d -a`
error: The following required arguments were not provided:
    -c
    <-a|-b>

USAGE:
    polobot -c -d -a <-a|-b>

For more information try --help
error: Process didn't exit successfully: `target/debug/test -d -a` (exit code: 1)
```


---

_Comment by @kbknapp on 2016-07-31 17:16_

Ah ok, thanks! I think I know where the issue is stemming from now :wink:


---

_Label `P1: urgent` added by @kbknapp on 2016-08-20 22:02_

---

_Label `P2: need to have` removed by @kbknapp on 2016-08-20 22:02_

---

_Added to milestone `2.10.1` by @kbknapp on 2016-08-20 22:13_

---

_Comment by @kbknapp on 2016-08-20 22:38_

Closed with #619 


---

_Closed by @kbknapp on 2016-08-20 22:38_

---

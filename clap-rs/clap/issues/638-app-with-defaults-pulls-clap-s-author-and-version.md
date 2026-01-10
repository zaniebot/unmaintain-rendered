---
number: 638
title: "`App::with_defaults` pulls clap's author and version"
type: issue
state: closed
author: guiniol
labels:
  - C-bug
  - A-builder
  - E-hard
assignees: []
created_at: 2016-08-28T11:20:24Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/638
synced_at: 2026-01-10T01:26:33Z
---

# `App::with_defaults` pulls clap's author and version

---

_Issue opened by @guiniol on 2016-08-28 11:20_

Using the new `App:with_defaults` displays clap's author and version number.
This is with clap version 2.11.0


---

_Comment by @kbknapp on 2016-08-28 17:33_

Interesting, I hadn't thought about that happenig. Thanks for reporting!


---

_Label `T: bug` added by @kbknapp on 2016-08-28 19:16_

---

_Label `P1: urgent` added by @kbknapp on 2016-08-28 19:16_

---

_Label `C: app` added by @kbknapp on 2016-08-28 19:16_

---

_Label `D: easy` added by @kbknapp on 2016-08-28 19:16_

---

_Label `W: 2.x` added by @kbknapp on 2016-08-28 19:16_

---

_Label `C: macros` added by @kbknapp on 2016-08-28 19:16_

---

_Added to milestone `2.11.1` by @kbknapp on 2016-08-28 19:16_

---

_Added to milestone `2.12.0` by @kbknapp on 2016-09-05 20:30_

---

_Removed from milestone `2.11.1` by @kbknapp on 2016-09-05 20:30_

---

_Closed by @homu on 2016-09-05 22:03_

---

_Comment by @clux on 2016-09-07 22:48_

This still happens in 2.11.2, maybe that commit was meant to close #643 instead?


---

_Comment by @kbknapp on 2016-09-08 19:45_

@clux yes, that was a mistake. Thanks!


---

_Reopened by @kbknapp on 2016-09-08 19:45_

---

_Removed from milestone `2.12.0` by @kbknapp on 2016-09-10 19:45_

---

_Label `D: hard` added by @kbknapp on 2016-09-12 18:40_

---

_Label `D: easy` removed by @kbknapp on 2016-09-12 18:40_

---

_Referenced in [clap-rs/clap#684](../../clap-rs/clap/issues/684.md) on 2016-10-09 15:34_

---

_Comment by @kbknapp on 2016-10-09 15:35_

From @RoPP 

> Cargo.toml : 

```
[package]
name = "plop"
version = "0.1.0"
authors = ["Plop <plop@plop.net>"]

[dependencies]
clap = "2.9"
```

> src/main.rs :

``` rust
extern crate clap;
use clap::App;

fn main() {
    App::with_defaults("plop").get_matches();
}
```

``` shell
cargo run -- --help
     Running `target/debug/plop --help`
Plop 2.14.0        <-------------------
Kevin K. <kbknapp@gmail.com>  <--------

USAGE:
    plop

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

```


---

_Comment by @nabijaczleweli on 2016-10-15 14:14_

After a couple of iterations I've approached this from a different angle: what if instead the above `src/main.rs` looked like this:

``` rust
#[macro_use]
extern crate clap;
use clap::App;

fn main() {
    defaulted_app!("plop").get_matches();
}
```

Name to be improved, of course.


---

_Comment by @kbknapp on 2016-10-16 03:15_

@nabijaczleweli perhaps. I'm not against that, but also don't want to confuse people with the `clap_app!` macro, or macro builders.

At this point, I'm also not really against simply deprecating `App::with_defaults` as broken until we can decide something more elegant during the v3 build up. Because I think the with_defaults doesn't add a whole ton, and can be _very_ easily worked around by using `crate_version!` and `crate_authors!`


---

_Comment by @nabijaczleweli on 2016-10-16 06:32_

`App::with_defaults()` in its current state has 100% _no chance_ of working and tbh I don't really see its value, either.


---

_Comment by @kbknapp on 2016-10-16 19:02_

Agreed.


---

_Referenced in [clap-rs/clap#692](../../clap-rs/clap/pulls/692.md) on 2016-10-16 19:22_

---

_Closed by @homu on 2016-10-17 04:00_

---

_Comment by @rtaycher on 2016-10-28 11:26_

Darn it, I guess if it doesn't work it doesn't work.

Can I make a ticket to discuss a redesign for v3?


---

_Comment by @kbknapp on 2016-10-28 14:30_

Yeah its deprecated as basically unfixable at this time. :(

On Oct 28, 2016 7:26 AM, "Roman A. Taycher" notifications@github.com
wrote:

> Darn it, I guess if it doesn't work it doesn't work.
> 
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> https://github.com/kbknapp/clap-rs/issues/638#issuecomment-256897413,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AGntttf68UczkpzFhPLif23H984eG0U1ks5q4dvNgaJpZM4Ju5rb
> .


---

_Comment by @rtaycher on 2016-11-01 07:16_

It's because it pulls the info from the compilation of the clap-rs crate right?

And a macro like @nabijaczleweli proposed would work, right?

I was wondering if I could start a ticket to discuss re-design or if there's another place I could put it(forum?)?

I had some other ideas, like possibly putting CARGO_PKG_DESCRIPTION in about.


---

_Comment by @kbknapp on 2016-11-01 16:17_

@rtaycher I'm all for opening an issue/PR which either fixes this, or discusses new ways to accomplish it. There are just other issues which are are a higher priority for myself right now, but if you'd like to open the discussion and try out some fixes I'm all for it :+1: 


---

_Comment by @ishitatsuyuki on 2016-12-17 07:29_

#778 

---

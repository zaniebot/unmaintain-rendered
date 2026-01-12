```yaml
number: 826
title: "error: failed to select a version for `libc` (required by `appname`):"
type: issue
state: closed
author: CBSears
labels:
  - A-meta
assignees: []
created_at: 2017-01-28T18:04:43Z
updated_at: 2018-08-02T03:30:00Z
url: https://github.com/clap-rs/clap/issues/826
synced_at: 2026-01-12T16:14:09Z
```

# error: failed to select a version for `libc` (required by `appname`):

---

_@CBSears_

rustc 1.15.0-beta.3 (a035041ba 2017-01-07)
clap = "2.20.0"

I'm developing on OSX, latest and greatest.

I am only just starting to use clap. So I'm including essentially three lines in my project and I can't even get it to build. I'm not even calling clap.

In my Cargo.toml:

clap = "2.20.0"
libc = "0.2.20"

I've also tried libc 0.2.19 and 0.2.18 as well with the same error message. When I build my code:

% cargo clean; cargo build
    Updating registry `https://github.com/rust-lang/crates.io-index`
error: failed to select a version for `libc` (required by `appname`):
all possible versions conflict with previously selected versions of `libc`
  version 0.2.19 in use by libc v0.2.19
  possible versions to select: 0.2.20

In particular, this line makes no sense to me:

  version 0.2.19 in use by libc v0.2.19

My entire Cargo.toml is:

[package]
name = "appname"
version = "0.1.0"
authors = ["chris"]

build = "build.rs"

[dependencies]

time = "0.1"
clap = "2.20.0"
libc = "0.2.20"
rustc-serialize = "0.3"


---

_Renamed from "error: failed to select a version for `libc` (required by `xxx`):" to "error: failed to select a version for `libc` (required by `appname`):" by @CBSears on 2017-01-28 18:26_

---

_Comment by @CBSears on 2017-01-28 19:04_

The problem persisted *after* I commented out "extern crate clap;" and "clap = "2.20.0"
It went away after cargo update. But returned when I uncommented these lines.


---

_Comment by @kbknapp on 2017-01-29 22:53_

Thanks for reporting this! I saw this mentioned on the subreddit as well. I'll do some tests and see where it lies, if it's as simple as bumping the pinned libc version I'll do that.

---

_Label `C: meta` added by @kbknapp on 2017-01-29 22:53_

---

_Label `P1: urgent` added by @kbknapp on 2017-01-29 22:53_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-29 22:53_

---

_Comment by @kbknapp on 2017-01-29 23:40_

Alright this should be fixed, I'm uploading the PR now

---

_Comment by @CBSears on 2017-01-30 02:15_

Can you explain what it was since I wasn't understanding what the problem
was at all. Newish to Rust and I thought it was pilot error.

C

On Sun, Jan 29, 2017 at 3:40 PM Kevin K. <notifications@github.com> wrote:

> Alright this should be fixed, I'm uploading the PR now
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/kbknapp/clap-rs/issues/826#issuecomment-275955947>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AFJM0ReCOhPTlH6xQi2Kt9ecX0jWJl9Uks5rXSNdgaJpZM4Lwkkr>
> .
>
-- 
Ite Ursi


---

_Comment by @kbknapp on 2017-01-30 02:29_

To be honest I'm not 100% sure since `cargo` should allow for multiple versions of a dep to exist in the dep graph.

The way it went down is `clap` required exactly version `0.2.18` of `libc`, while one of `clap`s deps required "up to" version `0.2.20` and cargo didn't like that for some reason. 

I've relaxed `clap`s version requirements and also updated that dep of `clap`s ([term_size-rs](https://github.com/kbknapp/term_size-rs)) so it shouldn't happen again. Now both of those crates require anything semver compatible with `0.2.20` and above.

---

_Comment by @kbknapp on 2017-01-30 02:30_

As soon as #830 merges I'll put v2.20.1 on crates.io

---

_Comment by @kbknapp on 2017-01-30 20:31_

Closed with #830 

---

_Closed by @kbknapp on 2017-01-30 20:31_

---

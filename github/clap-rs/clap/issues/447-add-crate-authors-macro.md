---
number: 447
title: "add crate_authors! macro"
type: issue
state: closed
author: rtaycher
labels:
  - C-enhancement
assignees: []
created_at: 2016-03-11T17:44:29Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/447
synced_at: 2026-01-07T13:12:19-06:00
---

# add crate_authors! macro

---

_Issue opened by @rtaycher on 2016-03-11 17:44_

Once https://github.com/rust-lang/cargo/pull/2465 lands it would be useful to have a crate_authors! macro to complement the crate_version! macro (fetch authors from cargo).

It's a pretty small change.

https://github.com/kbknapp/clap-rs/compare/master...rtaycher:add_authors_macro

But it won't work on old cargo binaries.

Should I put this behind feature flags(at first)?


---

_Comment by @kbknapp on 2016-03-12 17:48_

Awesome idea! I'm ok with not using feature flags, so long as the minimal version of cargo for this macro is annotated in the documentation. Thanks :+1: 


---

_Label `T: enhancement` added by @kbknapp on 2016-03-12 17:49_

---

_Label `D: easy` added by @kbknapp on 2016-03-12 17:49_

---

_Label `P3: want to have` added by @kbknapp on 2016-03-12 17:49_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-12 17:49_

---

_Label `C: macros` added by @kbknapp on 2016-03-12 17:49_

---

_Comment by @sru on 2016-03-12 20:46_

Since the environment variable is new, we could provide two versions of macros: One is just using `env!` (which will emit compilation error if the environment variable is not found), and other using [`option_env!`](http://doc.rust-lang.org/stable/std/macro.option_env!.html) and let user provide default authors if the environment variable is not found.


---

_Comment by @rtaycher on 2016-03-12 22:58_

I thought about using option_env! instead but then you get something that will need to be deprecated or be awkward later on.

Also to clarify technically it's newer then new, it hasn't been merged yet though I think it will be soon. I was thinking about adding a feature flag then dropping the flag after a year.


---

_Comment by @sru on 2016-03-12 23:28_

@rtaycher I guess it can get awkward after a while.

Once the pr is merged, can you amend the commit messages to follow the contribution guideline and make a pull request?


---

_Comment by @kbknapp on 2016-03-13 00:04_

So yeah let's us a feature flag until it's in stable Rust. Whether that's the `unstable` flag or another one devoted to just this feature is the same to me.


---

_Comment by @TheNeikos on 2016-03-27 18:34_

I find it funny, it's exactly for this use case that I proposed that change on rust/cargo :smile: 


---

_Referenced in [clap-rs/clap#480](../../clap-rs/clap/pulls/480.md) on 2016-04-11 12:25_

---

_Referenced in [clap-rs/clap#482](../../clap-rs/clap/pulls/482.md) on 2016-04-11 23:38_

---

_Closed by @homu on 2016-04-17 22:52_

---

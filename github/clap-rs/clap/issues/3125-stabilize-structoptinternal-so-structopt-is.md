---
number: 3125
title: "Stabilize `StructOptInternal` so `StructOpt` is available to be implemented manually"
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:16:21Z
updated_at: 2021-12-09T16:40:54Z
url: https://github.com/clap-rs/clap/issues/3125
synced_at: 2026-01-07T13:12:19-06:00
---

# Stabilize `StructOptInternal` so `StructOpt` is available to be implemented manually

---

_Issue opened by @epage on 2021-12-09 16:16_

<a href="https://github.com/nagisa"><img src="https://avatars.githubusercontent.com/u/679122?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [nagisa](https://github.com/nagisa)**
_Monday Dec 30, 2019 at 16:38 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/317_

----

As per [this thread on reddit](https://www.reddit.com/r/rust/comments/efi4kb/who_else_depends_on_structopts_internal_api/fc0w1cu?utm_source=share&utm_medium=web2x) there’s intent to make public API version of `augment_clap`, but I don't believe I've seen an issue to that effect.

Alternatively `derive(StructOpt)` should support generics (i.e. the issue referenced in that thread should be reopened).


---

_Comment by @epage on 2021-12-09 16:16_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Monday Dec 30, 2019 at 19:03 GMT_

----

Yeah, I didn't bother to stabilize `augment_clap` just because nobody came here and asked "please do that" so I thought that nobody's really interested.

I agree we need to allow people implementing `StructOpt` manually; the question here is what the API should look like? 

We want to
* leave enough of elbow room for ourselves so we are able to implement new features (i.e the API must be expandable).
* make the API forward compatible, the code that have been working prior to the introducing of a new feature must continue to work properly. Should a user wish to use the new feature, they will be required to tweak their code (naturally).

I propose, after we have #314 landed:
* rename the `StructOptInternal` trait to `StructOptMachinery`  (the name is subject for bikeshedding).
* the new trait will become public, the methods will be set in stone.
* each of the methods must have a sensible default implementation (i.e works in most cases but not guaranteed to work in some of them). Default implementations are set in stone.
* users are free to re-implement the methods as they see fit,  `derive(StructOpt)` is free to re-implement the methods the way it wants.
* should we want a new feature that is not covered by the API existing at the moment, we can add new methods that have a default implementation that works in terms of the old API (i.e the default impl must be backward compatible, but not required to support the new feature). `derive(Structopt)` will re-implement it as required to support the new feature.

To everyone interested: you are free and *encouraged* to participate in the discussion. cc @TeXitoi 



> Alternatively `derive(StructOpt)` should support generics (i.e. the issue referenced in that thread should be reopened).

Like I said, I likely won't get to it on my own in foreseeable future. I'm reopening that issue and going to leave detailed description of the way we want it to be implemented. Such a PR will be accepted.


---

_Comment by @epage on 2021-12-09 16:27_

The new `Parser`, `Args`, etc traits are designed for people to implement, including supporting proper error reporting.

---

_Closed by @epage on 2021-12-09 16:27_

---

_Label `A-derive` added by @epage on 2021-12-09 16:40_

---

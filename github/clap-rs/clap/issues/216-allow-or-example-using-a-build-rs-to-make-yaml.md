---
number: 216
title: Allow, or example using a build.rs to make YAML just as performant as traditional
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2015-09-02T02:22:50Z
updated_at: 2018-08-02T03:29:43Z
url: https://github.com/clap-rs/clap/issues/216
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow, or example using a build.rs to make YAML just as performant as traditional

---

_Issue opened by @kbknapp on 2015-09-02 02:22_

Based on [this comment](https://www.reddit.com/r/rust/comments/3j9es2/clap_13_build_a_cli_from_yaml/cunnshc) in the subreddit, I'd like to look at using a `build.rs` to generate traditional code. This may also help #202 as well. 


---

_Label `T: enhancement` added by @kbknapp on 2015-09-02 02:23_

---

_Label `D: intermediate` added by @kbknapp on 2015-09-02 02:23_

---

_Label `P3: want to have` added by @kbknapp on 2015-09-02 02:23_

---

_Label `C: parsing` added by @kbknapp on 2015-09-02 02:23_

---

_Label `C: examples` added by @kbknapp on 2015-09-02 02:23_

---

_Comment by @sru on 2015-09-02 18:10_

The integration of code from build.rs is really clunky ([Cargo's guide to code generation in build scripts](http://doc.crates.io/build-script.html#case-study:-code-generation)), and it requires the user to actually supply build script.

I feel like compiler plugin is better approach, but it is probably going to take months to stabilize it...

---

Another suggestion:
This is like a spinoff from parsing YAML and [this comment on reddit by vks_](https://www.reddit.com/r/rust/comments/3j9es2/clap_13_build_a_cli_from_yaml/cuo4lgb). Since most Rust program that uses Cargo will have sensible Cargo.toml, if we are going to have YAML parsed in compile time, why not add parsing from Cargo.toml as an addition. The user then only needs to worry about parsing arguments.


---

_Comment by @kbknapp on 2015-09-02 18:27_

@sru I agree, but my thought process is to _allow_ it, but not mandate it. So everything will still work just like it does today...but if someone wants to take the time to use a `build.rs` they'll get some extra performance. 

If possible, I'd like to make that process of writing the `build.rs` as painless as possible. Perhaps by having a `clap_codegen` crate...I'm not sure.

The TOML question is answered on reddit, but #81 was on the todo list, but got taken off (at least for the time being) because TOML isn't really set up to nest like `clap` does when it comes to args and subcommands.


---

_Referenced in [clap-rs/clap#217](../../clap-rs/clap/issues/217.md) on 2015-09-02 21:53_

---

_Comment by @kbknapp on 2017-04-05 18:36_

Closed in favor of the `serde` transition. Will re-address once that's complete

---

_Closed by @kbknapp on 2017-04-05 18:36_

---

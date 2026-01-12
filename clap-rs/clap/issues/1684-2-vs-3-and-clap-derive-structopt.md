```yaml
number: 1684
title: 2 vs 3 and clap_derive/structopt
type: issue
state: closed
author: sollyucko
labels: []
assignees: []
created_at: 2020-02-09T21:36:11Z
updated_at: 2020-03-24T03:15:23Z
url: https://github.com/clap-rs/clap/issues/1684
synced_at: 2026-01-12T16:14:11Z
```

# 2 vs 3 and clap_derive/structopt

---

_@sollyucko_

### Affected Version of clap

2.33.0 vs 3.0.0-alpha.1

### Expected Behavior Summary

`use clap::Clap` should work on latest version from `cargo`, b/c used in `README.md`.

### Reason for Behavior

`README.md` documents `3.0.0-alpha.1`, but `cargo` only has up through `2.33.0`.

### Proposed Fix

Add some sort of warning to the top of `README.md`, preferably with one or more of the following:

- a link to the `2.x` `README.md`
- a way to install `3.x` (a dependency with a git url?)

### Keywords (to make the issue easier to search for)

- ``error[E0432]: unresolved import `clap::Clap` ``
- ``no `Clap` in the root``
- ``error: cannot determine resolution for the derive macro `Clap` ``
- ``note: import resolution is stuck, try simplifying macro imports``
- ``error: cannot find attribute `clap` in this scope``
- ``error[E0599]: no function or associated item named `parse` found for struct `Opts` in the current scope``
- ``function or associated item `parse` not found for this``
- ``function or associated item not found in `Opts` ``

---

_Referenced in [clap-rs/clap#1685](../../clap-rs/clap/pulls/1685.md) on 2020-02-10 11:47_

---

_Closed by @bors[bot] on 2020-02-10 23:14_

---

_Closed by @bors[bot] on 2020-02-10 23:14_

---

_Comment by @KennethanCeyer on 2020-03-17 15:00_

Thank you, I spent 2 hours to find a solution
This post is not found by Google search.

We've once again experienced how important the additional explanations are in the README.md Quick Guide and how this affects the Rust Newbies' first start.

I am a newbie, but I will try to contribute through PR when this happens.


---

_Comment by @pksunkara on 2020-03-17 16:01_

Which is why we want to release a beta version of v3 so people could start using it immediately without trying to work with v2. /cc @Dylan-DPC 

---

_Comment by @KennethanCeyer on 2020-03-17 18:16_

@pksunkara 
Oh yes, that's good news.
I should upgrade and use it too.

---

_Comment by @chipsenkbeil on 2020-03-24 03:15_

@pksunkara , beta release would be fantastic. The derive functionality is splendid and made my codebase a lot cleaner. Been working on a project since November that I'd like to publish, but blocked because I'm leveraging the derive API from clap 3.0.0-beta.1 which isn't on crates.io. Don't want to make the repository public until I can publish as I want to avoid the small possibility that someone squats on the name.

---

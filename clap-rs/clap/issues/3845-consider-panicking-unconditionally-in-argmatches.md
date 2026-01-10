---
number: 3845
title: "Consider panicking unconditionally in ArgMatches::get_arg"
type: issue
state: closed
author: stepancheg
labels:
  - C-enhancement
assignees: []
created_at: 2022-06-16T21:45:30Z
updated_at: 2022-06-16T21:58:08Z
url: https://github.com/clap-rs/clap/issues/3845
synced_at: 2026-01-10T01:27:47Z
---

# Consider panicking unconditionally in ArgMatches::get_arg

---

_Issue opened by @stepancheg on 2022-06-16 21:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.8

### Describe your use case

I've spend some time figuring out why my program did not fail locally, but failed in (very slow) CI.

It was because of this debug-only assertion:

https://github.com/clap-rs/clap/blob/5fdc26ee9ad3edd47b516b27460dec876c09f98e/src/parser/matches/arg_matches.rs#L1263-L1280

This is not perf-critical code, so probably better to make assertion unconditional.

### Describe the solution you'd like

Unconditional assertion (or deprecated this function and add another which returns error).

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @stepancheg on 2022-06-16 21:45_

---

_Comment by @epage on 2022-06-16 21:58_

Its not just a question of whether the checks add overhead but us tracking state to panic adds overhead
- In debug builds
  - An `Id` is `(u64, String)`
  - `ArgMatches` contains a `Vec<String>` of all of the defined ids
- In release builds
  - An `Id` is a `u64`
  - We do not track all of the defined ids

We will be changing the definition of `Id` in clap 4 but I doubt we will track around the `Vec` for error reporting purposes. 

In addition, clap is generally setup for reporting development errors in debug builds.  It was by happenstance that some of the new panics in 3.2 are in release builds and we may re-evaluate that.

---

_Closed by @epage on 2022-06-16 21:58_

---

_Referenced in [clap-rs/clap#3847](../../clap-rs/clap/issues/3847.md) on 2022-06-17 12:27_

---

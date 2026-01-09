---
number: 3365
title: "Unclear how to handle subcommands with `clap_man`"
type: issue
state: open
author: epage
labels:
  - C-bug
  - S-waiting-on-design
  - A-man
assignees: []
created_at: 2022-01-28T21:57:33Z
updated_at: 2022-11-23T18:00:02Z
url: https://github.com/clap-rs/clap/issues/3365
synced_at: 2026-01-07T13:12:19-06:00
---

# Unclear how to handle subcommands with `clap_man`

---

_Issue opened by @epage on 2022-01-28 21:57_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

v3.0.6

### Minimal reproducible code

TBD

### Steps to reproduce the bug with the above code

TBD

### Actual Behaviour

We show a list of subcommand man apges

### Expected Behaviour

TBD

### Additional Context

Split out of #3174

#1334 is relevant

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2022-01-28 21:57_

---

_Label `S-waiting-on-design` added by @epage on 2022-01-28 21:57_

---

_Label `A-man` added by @epage on 2022-01-28 21:57_

---

_Comment by @sondr3 on 2022-01-29 10:46_

For some design "inspiration" on how to lay it out, [`cabal`](https://man.archlinux.org/man/cabal.1) (cargo for Haskell) has everything in a single, gigantic document while [`cargo`](https://man.archlinux.org/man/cargo.1) and [`git`](https://man.archlinux.org/) lists the available subcommands and where to find them. I highly prefer the latter approach, but it shouldn't be too hard to support both.

---

_Comment by @epage on 2022-01-29 23:42_

In general, clap behaves more like most of git.  The linked issue (#1334) is about offering it to work more like `git stash`'s subcommands.

Its also something we'd want to have an example for to help show how to go about it and to verify we have the full experience worked out,.

---

_Comment by @gibfahn on 2022-06-15 11:32_

Wanted to link the only example I've found (maybe I missed an obvious one) of a way to handle subcommand man page generation:

https://github.com/CKingX/ddrescue_error_mapping/blob/05899edbe60c0ee6cfdda70aa6379161d2fb1390/build.rs#L20-L38

---

_Comment by @sunshowers on 2022-06-25 20:46_

@gibfahn for [cargo-nextest](https://nexte.st/) I [tried that approach](https://github.com/nextest-rs/nextest/pull/312) but encountered https://github.com/clap-rs/clap/issues/3869 unfortunately.

---

_Referenced in [NNPDF/pineappl#145](../../NNPDF/pineappl/issues/145.md) on 2022-07-28 13:14_

---

_Referenced in [nihaals/clap-complete-command#11](../../nihaals/clap-complete-command/pulls/11.md) on 2022-11-23 17:51_

---

_Comment by @nihaals on 2022-11-23 18:00_

It would be nice if there was a way to switch between referencing pages for subcommands and including them in the same page (as either an argument to `Man::render()` or a different method) so you could switch between the two approaches (like `cabal` and `git` as [mentioned above](https://github.com/clap-rs/clap/issues/3365#issuecomment-1024887176)), as in some contexts having a single page might fit better. It would also provide an immediate workaround for #3603 where a different API wouldn't need to be worked on yet.

---

_Referenced in [clap-rs/clap#4818](../../clap-rs/clap/issues/4818.md) on 2023-04-03 20:18_

---

_Referenced in [stephen-bunn/artsum#2](../../stephen-bunn/artsum/pulls/2.md) on 2025-11-15 12:30_

---

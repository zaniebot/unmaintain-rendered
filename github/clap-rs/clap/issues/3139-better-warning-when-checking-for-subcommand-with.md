---
number: 3139
title: better warning when checking for subcommand with is_matches()
type: issue
state: closed
author: matthiaskrgr
labels:
  - C-enhancement
assignees: []
created_at: 2021-12-10T00:03:57Z
updated_at: 2021-12-10T01:40:24Z
url: https://github.com/clap-rs/clap/issues/3139
synced_at: 2026-01-07T13:12:19-06:00
---

# better warning when checking for subcommand with is_matches()

---

_Issue opened by @matthiaskrgr on 2021-12-10 00:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-rc.1

### Describe your use case

I started looking into upgrading `cargo-cache` https://github.com/matthiaskrgr/cargo-cache to clap 3 and right at the clap started crashing it because I was checking for undefined args which is great because it found a couple of bugs. :)

I had a couple of cases where I defined a subcommand name `foo` and then later checked `ArgMatches.is_present("foo")`, essentially defining `cargo-cache foo` but then coding for `cargo-cache --foo` later on.

It's one of these "how could that ever work??" moments :laughing: 

### Describe the solution you'd like

````
thread 'main' panicked at '`'local' is not a name of an argument or a group.`
````

It would be great if the clap runtime when it is passed a `--local` and does not expect  one, could also check if there is a `local` subcommand defined and print that in the suggestion:

`It looks like you have defined a "local" subcommand. Did you intend to code "subcommand_matches()" instead of "is_present()"?` 
or something like this :)

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @matthiaskrgr on 2021-12-10 00:03_

---

_Comment by @epage on 2021-12-10 00:49_

> It would be great if the clap runtime when it is passed a --local and does not expect one, could also check if there is a local subcommand defined and print that in the suggestion:

Good idea! Coding it up now

> I had a couple of cases where I defined a subcommand name foo and then later checked ArgMatches.is_present("foo"), essentially defining cargo-cache foo but then coding for cargo-cache --foo later on.

We actually used to support subcommands with `is_present`, that was one of the breaking changes in the release.

---

_Comment by @matthiaskrgr on 2021-12-10 01:05_

another small thing: I noticed the none-RUST_BACKTRACE=1 message showed the panic source as inside the clap source in the cargo cache.
I wonder if you can slap `#[track_caller]` on a bunch of things so that the displayed none-backtrace line already shows where the problem originates from in MY code (and not where the is a `panic` or whatever is in claps code :) )

EDIT: nvm just saw the PR already adds track caller :heart: 
looking forward to rc2 then :D 

---

_Closed by @epage on 2021-12-10 01:35_

---

_Comment by @epage on 2021-12-10 01:40_

And rc3 is out (messed up publishing rc2)

---

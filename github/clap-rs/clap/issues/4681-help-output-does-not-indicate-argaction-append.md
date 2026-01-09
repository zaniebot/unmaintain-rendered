---
number: 4681
title: "Help output does not indicate `ArgAction::Append`"
type: issue
state: open
author: cd-work
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2023-01-27T22:43:53Z
updated_at: 2023-01-31T13:50:20Z
url: https://github.com/clap-rs/clap/issues/4681
synced_at: 2026-01-07T13:12:20-06:00
---

# Help output does not indicate `ArgAction::Append`

---

_Issue opened by @cd-work on 2023-01-27 22:43_

Maintainer's note: any proposed syntax should include a mockup of `cargo check -h` ([details](https://github.com/clap-rs/clap/issues/4681#issuecomment-1410392153))

---

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.4

### Describe your use case

To my knowledge, the correct way to allow an argument to be specified multiple times is to use `ArgAction::Append`. This correctly allows passing the option multiple times and returns a `Vec` with `get_many`.

However, when getting the `--help` output it looks like this:

```
Options:
  -o, --option <OPTION>   My Option
```

It seems like the `--help` output does not communicate at all that the option can be passed multiple times.

### Describe the solution you'd like

Considering that `--option <OPTION>...` is already used to indicate that one option can has multiple arguments, the best solution I can see would be `--option... <OPTION>`. This would be consistent with `--flag...`.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @cd-work on 2023-01-27 22:43_

---

_Referenced in [phylum-dev/cli#927](../../phylum-dev/cli/pulls/927.md) on 2023-01-27 22:44_

---

_Label `A-help` added by @epage on 2023-01-27 22:49_

---

_Label `S-waiting-on-design` added by @epage on 2023-01-27 22:49_

---

_Comment by @epage on 2023-01-27 22:51_

More options are being discussed in https://github.com/clap-rs/clap/discussions/3272.   

Personally, I think `--open... <OPTION>` isn't too clear.

---

_Comment by @cd-work on 2023-01-28 00:03_

> Personally, I think `--open... <OPTION>` isn't too clear.

That's a fair point, but I think it's one of the least offensive options considering the existing output. But yeah I can see how people might struggle to understand its exact purpose, I'm just looking for **any** indication to the user that this isn't a normal flag.

---

_Comment by @maxrake on 2023-01-30 17:41_

This community is likely already aware of it, but there is an [IEEE Std 1003.1-2017 for Utility Conventions](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) that describes the argument syntax for standard utilities used throughout POSIX. It covers the use of ellipses:

> Ellipses ( "..." ) are used to denote that one or more occurrences of an operand are allowed. When an option or an operand followed by ellipses is enclosed in brackets, zero or more options or operands can be specified. The form:
`utility_name [-g option_argument]... [operand...]`
indicates that multiple occurrences of the option and its option-argument preceding the ellipses are valid, with semantics as indicated in the OPTIONS section of the utility. (See also Guideline 11 in [Utility Syntax Guidelines](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html#tag_12_02) .)
The form:
`utility_name -f option_argument [-f option_argument]... [operand...]`
indicates that the -f option is required to appear at least once and may appear multiple times.

Granted, this syntax is meant for the `Usage:` section and this issue appears to be discussing how to represent the format in the `Options:` section of the help output. Still, with that in mind, the `--option... <OPTION_ARGUMENT>` format appears to be a close match.

---

_Comment by @epage on 2023-01-30 17:54_

`[-g option_argument]` came up in #3272 in a couple of threads.

Quoting myself from that thread

> `[-S Rev]...` doesn't quite work as we have both the short and long being shown. I also worry it could get quite noisy.

As for that being a cause to use `--option... <OPTION_ARGUMENT>`, I don't feel these are connected enough to draw that conclusion.

---

_Comment by @epage on 2023-01-30 17:54_



Pulling in another idea from #3272 is putting `[+]` after the short/long in the argument list which is what at least `hg` does.  My main concern is how well people will understand what it means.  I'd like to see more examples of who all uses it.

---

_Comment by @cd-work on 2023-01-31 09:34_

> Pulling in another idea from https://github.com/clap-rs/clap/discussions/3272 is putting [+] after the short/long in the argument list which is what at least hg does. My main concern is how well people will understand what it means. I'd like to see more examples of who all uses it.

Honestly just my 2 cents but I **really** am not a fan of `[+]`. It's entirely unheard of in the CLI for me and does not fit in with the rest of clap's indicators.

I think what @maxrake suggested with `[-t, --test <OPTION>]...` is the most sensible and uncontroversial option (no need to reinvent the wheel).

---

_Comment by @epage on 2023-01-31 13:49_

I think context will be important for this thread, so I'm going to ask that any proposal for new output syntax include a mockup of `cargo check -h` with the syntax.

Arguments affected by this discussion include
- `--package`
- `--exclude`
- `--bin`
- `--example`
- `--features`
- `--target`
- `--mesage-format`

<details>
<summary>Current output:</summary>

```console
$ cargo check -h
Check a local package and all of its dependencies for errors

Usage: cargo check [OPTIONS]

Options:
  -q, --quiet                   Do not print cargo log messages
  -p, --package [<SPEC>]        Package(s) to check
      --workspace               Check all packages in the workspace
      --exclude <SPEC>          Exclude packages from the check
  -v, --verbose...              Use verbose output (-vv very verbose/build.rs output)
      --all                     Alias for --workspace (deprecated)
      --color <WHEN>            Coloring: auto, always, never
  -j, --jobs <N>                Number of parallel jobs, defaults to # of CPUs
      --frozen                  Require Cargo.lock and cache are up to date
      --keep-going              Do not abort the build as soon as there is an error (unstable)
      --lib                     Check only this package's library
      --locked                  Require Cargo.lock is up to date
      --bin [<NAME>]            Check only the specified binary
      --offline                 Run without accessing the network
      --bins                    Check all binaries
      --config <KEY=VALUE>      Override a configuration value
      --example [<NAME>]        Check only the specified example
  -Z <FLAG>                     Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details
      --examples                Check all examples
      --test [<NAME>]           Check only the specified test target
      --tests                   Check all tests
      --bench [<NAME>]          Check only the specified bench target
      --benches                 Check all benches
      --all-targets             Check all targets
  -r, --release                 Check artifacts in release mode, with optimizations
      --profile <PROFILE-NAME>  Check artifacts with the specified profile
  -F, --features <FEATURES>     Space or comma separated list of features to activate
      --all-features            Activate all available features
      --no-default-features     Do not activate the `default` feature
      --target <TRIPLE>         Check for the target triple
      --target-dir <DIRECTORY>  Directory for all generated artifacts
      --manifest-path <PATH>    Path to Cargo.toml
      --ignore-rust-version     Ignore `rust-version` specification in packages
      --message-format <FMT>    Error format
      --unit-graph              Output build graph in JSON (unstable)
      --future-incompat-report  Outputs a future incompatibility report at the end of the build
      --timings[=<FMTS>]        Timing output formats (unstable) (comma separated): html, json
  -h, --help                    Print help information

Run `cargo help check` for more detailed information.
```

</details>

---

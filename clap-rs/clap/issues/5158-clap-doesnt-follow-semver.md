---
number: 5158
title: clap doesnt follow semver
type: issue
state: closed
author: labannah9125
labels:
  - C-bug
assignees: []
created_at: 2023-10-04T18:15:31Z
updated_at: 2023-10-04T18:21:39Z
url: https://github.com/clap-rs/clap/issues/5158
synced_at: 2026-01-10T01:28:07Z
---

# clap doesnt follow semver

---

_Issue opened by @labannah9125 on 2023-10-04 18:15_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.68

### Clap Version

4.4.5

### Minimal reproducible code

any of use of https://github.com/clap-rs/clap/blob/v4.4.6/CHANGELOG.md#compatibility or later

### Steps to reproduce the bug with the above code

n/a

### Actual Behaviour

compilation errors about msrv when the compiler is older than specified in same major version

### Expected Behaviour

msrv should change with major version per semver to not break older compilers using clap.

### Additional Context

the top of the changelog says the project uses semver and links to the rules but then violated them multiple times in the 4.x series. bumping minor vers will break program keeping updated deps on systems using os packages for rust that dont move fast like debian or openbsd

### Debug Output

_No response_

---

_Label `C-bug` added by @labannah9125 on 2023-10-04 18:15_

---

_Comment by @epage on 2023-10-04 18:21_

The cargo guidelines suggest that changing the MSRV is a minor incompatibility, not a major incompatibility, see https://doc.rust-lang.org/cargo/reference/semver.html#env-new-rust

While there isn't as firm statement in any location (yet), this approach has effectively become the de facto standard within the Rust ecosystem and you can see it used pervasively.

Put another way, MSRV is just a subset of "build environment" and there are a lot of build environent changes that have not caused major version bumps.  For example, rustc regularly does things like require new versions of macOS, upgraded Android NDKs, new glibc's, etc.

As we are following our documented policy and the general community approach, I'm closing this.

btw if you had looked, we have several existing closed issues for this, like #5145.

---

_Closed by @epage on 2023-10-04 18:21_

---

_Referenced in [Sovereign-Labs/sovereign-sdk#1108](../../Sovereign-Labs/sovereign-sdk/pulls/1108.md) on 2023-10-25 10:28_

---

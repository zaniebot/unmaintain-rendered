---
number: 3267
title: Please lower the MSRV
type: issue
state: closed
author: djc
labels:
  - C-enhancement
  - E-medium
  - A-meta
  - E-help-wanted
assignees: []
created_at: 2022-01-07T12:38:04Z
updated_at: 2023-12-30T12:51:18Z
url: https://github.com/clap-rs/clap/issues/3267
synced_at: 2026-01-10T01:27:37Z
---

# Please lower the MSRV

---

_Issue opened by @djc on 2022-01-07 12:38_

Maintainer's notes

[Disposition comment](https://github.com/clap-rs/clap/issues/3267#issuecomment-1010171427)
> How about we give this a trial. If a contributor is willing to step in and create PRs to make all of the dependencies work on the proposed MSRV and they get accepted by the maintainers, we can provisionally (and unofficially) lower the MSRV. After a period of time (6 months?), we can re-evaluate and possibly adjust the MSRV policy. If there are problems with the provisional MSRV, we reserve the right to revert it, even in a patch release (hence the unofficial designation). Depending on what happened, a contributor can step in to help lower it again and we can consider trying this again.

To clarify, yes, we can check this unofficial, provisional MSRV in CI
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.5

### Describe your use case

Currently I'd like to use clap (with derive) in the example code for the rustls-mio crate. However, the rustls repo currently has CI set to a 1.52.1 MSRV (and realistically I'd prefer to stick with 1.51 or even 1.50).

### Describe the solution you'd like

I'd like the MSRV to be ideally no younger than 1.50. 1.51 would also be okay, maybe 1.52.

### Alternatives, if applicable

_No response_

### Additional Context

So far I found the `#![doc = include_str!()]` usage in `clap_derive` and the use of `os_str_bytes` which currently requires 1.52 (but apparently only [because](https://github.com/dylni/os_str_bytes/commit/6acb7e6e4233948ffe855af4079ad3355350d0db) it wants to support some lint, which seems like a pretty bad reason). I'm not sure how important os_str_bytes is to this crate's mechanics; maybe relying on the platform-specific `OsStrExt` implementations could go a long way?

---

_Label `C-enhancement` added by @djc on 2022-01-07 12:38_

---

_Referenced in [rustls/rustls#879](../../rustls/rustls/pulls/879.md) on 2022-01-07 12:49_

---

_Comment by @epage on 2022-01-07 14:23_

I can understand the limitation of clap3's MSRV making it more difficult to update from clap2.  

This request is to lower the MSRV to a specific number but what I'm wondering is how this affects our MSRV policy.  Clap's MSRV policy has always been ["2 back"](https://github.com/clap-rs/clap/#aspirations) (though without any non-patch releases in a long time, it has artificially extended the MSRV).  How long are you requesting we stay on 1.52 or earlier?  What would we be able to upgrade to after that?  Effectively, is this a one-time request or a request to change our MSRV policy to something else and, if so, what?

---

_Comment by @epage on 2022-01-07 14:24_

Forgot to add, `os_str_bytes` does a **lot** of heavy lifting for clap.  Previously, the idea was proposed to drop it to reduce dependencies (and this compile times / binary size) but was [was rejected due to its high value](https://github.com/clap-rs/clap/issues/1365#issuecomment-622650802).

---

_Comment by @djc on 2022-01-08 22:03_

I guess it's more about the policy. IMO clap is an important crate which, apart from CLIs which might arguably not have as strict MSRV requirements, is also used often in examples for library projects. (For example, Quinn and Trust-DNS are libraries currently using structopt in their repositories -- rustls has been using docopt, which is unmaintained.)

As such, I think a more conservative MSRV policy would be appropriate for this project. MSRV bumps should come with sufficient justification unless the bump is about a very old release (so the bump in os_str_bytes for the sole purpose of enabling some lint looks bad to me). For Quinn (currently at 1.51.0), we try to match the Tokio project which has a 6 month policy but actually ends up more conservative (currently at 1.46.0).

Some recent discussion: https://twitter.com/mitsuhiko/status/1479832038257704964

---

_Comment by @epage on 2022-01-10 17:26_

I feel like this is another example of the curse of clap3 taking too long :smile: .  The other being a subset of people who basically want the same time span before future breaking changes.

I assume those crates have to hold back dependencies.  Is there a reason they are not wanting to hold clap back but upgrade?  Would a temporary lowering of MSRV for upgrade but then a resuming of regular policy meet those needs?

We have competing care abouts from maintainers (access to functionality and fixes, low process overhead) and different user bases (accelerated development vs low churn in API breaks of MSRV).

I would say our primary target user is the CLI developer who is wanting further improvements at the cost of churn.

---

A breakdown of challenges:

So as alluded to, we are constrained by our dependencies.  There are tiers to this:
- Do we have different MSRVs for different feature sets?
- Do we only check compilation on the MSRV or do we support tests?
- Do we have different MSRVs for different crates in the workspace?

If we supported testing and all features on all crates, that greatly increases the number of dependencies (see below).

The easiest to verify is if we support all features on all targets on all crates.  Different slices and dices of this get messy.  We'd end up having to hold back more dependencies for MSRV.

We also have to balance holding back dependency upgrades with our users who are trying to minimize the number of duplicate dependencies (e.g. https://github.com/TeXitoi/structopt/pull/518).  This also limits what features and bug fixes we get access to in those dependencies or from the dependencies we can't use because of the policy.

Dependencies aren't the only consideration.  The proposal would basically require us to prepare to litigate whether an MSRV upgrade is justified (and possibly go through those conversations like is partially happening here) for various user bases which aren't our primary target (CLIs).  

Any MSRV also introduces challenges and this would exacerbate it, like dealing with new lints that need to be silenced in macros.

Dependencies of just `clap` (`clap_man` and other future crates can balloon this):
```
clap v3.0.5 (/home/epage/src/personal/clap)
├── atty v0.2.14
│   └── libc v0.2.107
├── backtrace v0.3.63
│   ├── addr2line v0.17.0
│   │   └── gimli v0.26.1
│   ├── cfg-if v1.0.0
│   ├── libc v0.2.107
│   ├── miniz_oxide v0.4.4
│   │   └── adler v1.0.2
│   │   [build-dependencies]
│   │   └── autocfg v1.0.1
│   ├── object v0.27.1
│   │   └── memchr v2.4.1
│   └── rustc-demangle v0.1.21
│   [build-dependencies]
│   └── cc v1.0.72
├── bitflags v1.3.2
├── clap_derive v3.0.5 (proc-macro) (/home/epage/src/personal/clap/clap_derive)
│   ├── heck v0.4.0
│   ├── proc-macro-error v1.0.4
│   │   ├── proc-macro-error-attr v1.0.4 (proc-macro)
│   │   │   ├── proc-macro2 v1.0.32
│   │   │   │   └── unicode-xid v0.2.2
│   │   │   └── quote v1.0.10
│   │   │       └── proc-macro2 v1.0.32 (*)
│   │   │   [build-dependencies]
│   │   │   └── version_check v0.9.3
│   │   ├── proc-macro2 v1.0.32 (*)
│   │   ├── quote v1.0.10 (*)
│   │   └── syn v1.0.81
│   │       ├── proc-macro2 v1.0.32 (*)
│   │       ├── quote v1.0.10 (*)
│   │       └── unicode-xid v0.2.2
│   │   [build-dependencies]
│   │   └── version_check v0.9.3
│   ├── proc-macro2 v1.0.32 (*)
│   ├── quote v1.0.10 (*)
│   └── syn v1.0.81 (*)
├── indexmap v1.7.0
│   └── hashbrown v0.11.2
│   [build-dependencies]
│   └── autocfg v1.0.1
├── lazy_static v1.4.0
├── os_str_bytes v6.0.0
│   └── memchr v2.4.1
├── regex v1.5.4
│   ├── aho-corasick v0.7.18
│   │   └── memchr v2.4.1
│   ├── memchr v2.4.1
│   └── regex-syntax v0.6.25
├── strsim v0.10.0
├── termcolor v1.1.2
├── terminal_size v0.1.17
│   └── libc v0.2.107
├── textwrap v0.14.2
│   ├── terminal_size v0.1.17 (*)
│   └── unicode-width v0.1.9
├── unicase v2.6.0
│   [build-dependencies]
│   └── version_check v0.9.3
└── yaml-rust v0.4.5
    └── linked-hash-map v0.5.4
[dev-dependencies]
├── criterion v0.3.5
│   ├── atty v0.2.14 (*)
│   ├── cast v0.2.7
│   │   [build-dependencies]
│   │   └── rustc_version v0.4.0
│   │       └── semver v1.0.4
│   ├── clap v2.33.3
│   │   ├── bitflags v1.3.2
│   │   ├── textwrap v0.11.0
│   │   │   └── unicode-width v0.1.9
│   │   └── unicode-width v0.1.9
│   ├── criterion-plot v0.4.4
│   │   ├── cast v0.2.7 (*)
│   │   └── itertools v0.10.1
│   │       └── either v1.6.1
│   ├── csv v1.1.6
│   │   ├── bstr v0.2.17
│   │   │   ├── lazy_static v1.4.0
│   │   │   ├── memchr v2.4.1
│   │   │   ├── regex-automata v0.1.10
│   │   │   └── serde v1.0.130
│   │   │       └── serde_derive v1.0.130 (proc-macro)
│   │   │           ├── proc-macro2 v1.0.32 (*)
│   │   │           ├── quote v1.0.10 (*)
│   │   │           └── syn v1.0.81 (*)
│   │   ├── csv-core v0.1.10
│   │   │   └── memchr v2.4.1
│   │   ├── itoa v0.4.8
│   │   ├── ryu v1.0.5
│   │   └── serde v1.0.130 (*)
│   ├── itertools v0.10.1 (*)
│   ├── lazy_static v1.4.0
│   ├── num-traits v0.2.14
│   │   [build-dependencies]
│   │   └── autocfg v1.0.1
│   ├── oorandom v11.1.3
│   ├── plotters v0.3.1
│   │   ├── num-traits v0.2.14 (*)
│   │   ├── plotters-backend v0.3.2
│   │   └── plotters-svg v0.3.1
│   │       └── plotters-backend v0.3.2
│   ├── rayon v1.5.1
│   │   ├── crossbeam-deque v0.8.1
│   │   │   ├── cfg-if v1.0.0
│   │   │   ├── crossbeam-epoch v0.9.5
│   │   │   │   ├── cfg-if v1.0.0
│   │   │   │   ├── crossbeam-utils v0.8.5
│   │   │   │   │   ├── cfg-if v1.0.0
│   │   │   │   │   └── lazy_static v1.4.0
│   │   │   │   ├── lazy_static v1.4.0
│   │   │   │   ├── memoffset v0.6.4
│   │   │   │   │   [build-dependencies]
│   │   │   │   │   └── autocfg v1.0.1
│   │   │   │   └── scopeguard v1.1.0
│   │   │   └── crossbeam-utils v0.8.5 (*)
│   │   ├── either v1.6.1
│   │   └── rayon-core v1.9.1
│   │       ├── crossbeam-channel v0.5.1
│   │       │   ├── cfg-if v1.0.0
│   │       │   └── crossbeam-utils v0.8.5 (*)
│   │       ├── crossbeam-deque v0.8.1 (*)
│   │       ├── crossbeam-utils v0.8.5 (*)
│   │       ├── lazy_static v1.4.0
│   │       └── num_cpus v1.13.0
│   │           └── libc v0.2.107
│   │   [build-dependencies]
│   │   └── autocfg v1.0.1
│   ├── regex v1.5.4 (*)
│   ├── serde v1.0.130 (*)
│   ├── serde_cbor v0.11.2
│   │   ├── half v1.8.2
│   │   └── serde v1.0.130 (*)
│   ├── serde_derive v1.0.130 (proc-macro) (*)
│   ├── serde_json v1.0.69
│   │   ├── itoa v0.4.8
│   │   ├── ryu v1.0.5
│   │   └── serde v1.0.130 (*)
│   ├── tinytemplate v1.2.1
│   │   ├── serde v1.0.130 (*)
│   │   └── serde_json v1.0.69 (*)
│   └── walkdir v2.3.2
│       └── same-file v1.0.6
├── lazy_static v1.4.0
├── regex v1.5.4 (*)
├── rustversion v1.0.5 (proc-macro)
├── trybuild v1.0.52
│   ├── glob v0.3.0
│   ├── lazy_static v1.4.0
│   ├── serde v1.0.130 (*)
│   ├── serde_json v1.0.69 (*)
│   ├── termcolor v1.1.2
│   └── toml v0.5.8
│       └── serde v1.0.130 (*)
└── trycmd v0.9.0
    ├── concolor-control v0.0.7
    │   ├── atty v0.2.14 (*)
    │   ├── bitflags v1.3.2
    │   └── concolor-query v0.0.4
    ├── difflib v0.4.0
    ├── escargot v0.5.7
    │   ├── log v0.4.14
    │   │   └── cfg-if v1.0.0
    │   ├── once_cell v1.8.0
    │   ├── serde v1.0.130 (*)
    │   └── serde_json v1.0.69 (*)
    ├── glob v0.3.0
    ├── humantime v2.1.0
    ├── humantime-serde v1.0.1
    │   ├── humantime v2.1.0
    │   └── serde v1.0.130 (*)
    ├── normalize-line-endings v0.3.0
    ├── os_pipe v1.0.0
    │   └── libc v0.2.107
    ├── rayon v1.5.1 (*)
    ├── serde v1.0.130 (*)
    ├── shlex v1.1.0
    ├── toml_edit v0.12.3
    │   ├── combine v4.6.2
    │   │   ├── bytes v1.1.0
    │   │   └── memchr v2.4.1
    │   ├── indexmap v1.7.0 (*)
    │   ├── itertools v0.10.1 (*)
    │   ├── kstring v1.0.6
    │   │   └── serde v1.0.130 (*)
    │   └── serde v1.0.130 (*)
    ├── wait-timeout v0.2.0
    │   └── libc v0.2.107
    └── yansi v0.5.0
```

---

_Comment by @djc on 2022-01-11 09:39_

I think the reality is that's quite feasible even if it might seem daunting at first. Let's review direct library dependencies for clap and clap_derive:

* atty: mostly hasn't been touched for years, so probably has a pretty conservative MSRV (Travis config doesn't test MSRV)
* backtrace: tests for 1.42
* bitflags: documented as 1.46
* heck: documented as 1.32
* proc-macro-error: documented as 1.31
* proc-macro2: CI tests 1.31
* quote: documented as 1.31
* syn: documented as 1.31
* indexmap: CI tests 1.49
* lazy_static: documented as 1.27
* os_str_bytes: documented as 1.52, discussed above
* regex: documented as 1.41
* strsim: CI tests 1.31
* termcolor: CI tests 1.34
* terminal_size: documented as 1.31
* textwrap: doesn't seem to document or test any MSRV
* unicase: last commit 2 years ago, Travis tests 1.31
* yaml-rust: documented as 1.31

In other words, the minimum bounds seem to be:

* 1.54 because of clap itself
* 1.52 because of os_str_bytes (but only for a lint)
* 1.49 because of indexmap
* 1.46 because of bitflags

> We have competing care abouts from maintainers (access to functionality and fixes, low process overhead) and different user bases (accelerated development vs low churn in API breaks of MSRV).

I don't think, with the current state of Rust, the MSRV really needs to constrain feature development; at least that's not my experience. There's almost always a straightforward fix, and if it will be pointed out by CI the contributor will likely quickly take care of it.

> The easiest to verify is if we support all features on all targets on all crates. Different slices and dices of this get messy. We'd end up having to hold back more dependencies for MSRV.

That seems fine, a single MSRV for the entire repository is what pretty much all projects do.

> We also have to balance holding back dependency upgrades with our users who are trying to minimize the number of duplicate dependencies (e.g. TeXitoi/structopt#518). This also limits what features and bug fixes we get access to in those dependencies or from the dependencies we can't use because of the policy.

I'm generally not asking you to hold back dependency upgrades (I always do them ASAP). When a dependency comes with a significant MSRV bump to something that's a little fresh for my projects' MSRV policy, I've sometimes gone upstream to request a reassessment of that change, and maintainers have usually been responsive to that kind of argument (especially for the kind of widely used dependencies you seem to have here).

> Dependencies aren't the only consideration. The proposal would basically require us to prepare to litigate whether an MSRV upgrade is justified (and possibly go through those conversations like is partially happening here) for various user bases which aren't our primary target (CLIs).

If you stick to 6-9 months (4-6 releases) I think it'll almost never be a big deal -- it certainly hasn't been for most of the projects I maintain.

---

_Comment by @epage on 2022-01-11 17:02_

How about we give this a trial.  If a contributor is willing to step in and create PRs to make all of the dependencies work on the proposed MSRV and they get accepted by the maintainers, we can provisionally (and unofficially) lower the MSRV.  After a period of time (6 months?), we can re-evaluate and possibly adjust the MSRV policy.  If there are problems with the provisional MSRV, we reserve the right to revert it, even in a patch release (hence the unofficial designation).  Depending on what happened, a contributor can step in to help lower it again and we can consider trying this again.

---

_Label `A-meta` added by @epage on 2022-01-11 17:08_

---

_Label `S-triage` added by @epage on 2022-01-11 17:08_

---

_Comment by @djc on 2022-01-12 09:45_

What do you mean by "provisionally (and unofficially)"? Do you mean you don't change the documentation about the MSRV? Can we still lower the MSRV in CI? Without changing it in CI setting an unofficial lower MSRV is pointless, as it will be a pain to figure out what changes are violating the unofficial MSRV.

I propose we make changes in this repository to support 1.52 as the unofficial MSRV and enforce this in CI. If there is a PR that runs into MSRV issues that violate an unofficial 6-month policy (which would actually be 1.53, but we get one release for free), I'm happy to jump in to see if there's a way out.

---

_Comment by @epage on 2022-01-12 15:40_

Provisional: this is on a trial bases, like 6 months

Unofficial: we reserve the right to change MSRV on patch releases.  We probably won't advertise this MSRV.  I agree about checking it in CI.

---

_Label `S-triage` removed by @epage on 2022-01-12 15:40_

---

_Label `E-medium` added by @epage on 2022-01-12 15:40_

---

_Label `E-help-wanted` added by @epage on 2022-01-12 15:40_

---

_Referenced in [hickory-dns/hickory-dns#1616](../../hickory-dns/hickory-dns/pulls/1616.md) on 2022-01-16 17:07_

---

_Referenced in [clap-rs/clap#3299](../../clap-rs/clap/issues/3299.md) on 2022-01-17 13:51_

---

_Referenced in [rust-lang/mdBook#1731](../../rust-lang/mdBook/pulls/1731.md) on 2022-01-19 16:27_

---

_Referenced in [quinn-rs/quinn#1287](../../quinn-rs/quinn/pulls/1287.md) on 2022-01-22 23:02_

---

_Comment by @djc on 2022-02-01 09:56_

Okay, I'm going to give up on reducing MSRV below 1.54 since it's now been 6 months anyway. Let's see if we can stick to 1.54 as the unofficial CI-checked MSRV for now, though.

---

_Closed by @djc on 2022-02-01 09:56_

---

_Referenced in [clap-rs/clap#3727](../../clap-rs/clap/issues/3727.md) on 2022-05-13 11:59_

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-17 22:06_

---

_Referenced in [rustls/pemfile#14](../../rustls/pemfile/issues/14.md) on 2023-02-15 13:21_

---

_Comment by @stevenroose on 2023-08-24 17:08_

clap's current MSRV is less than 2 months old.. Is it unreasonable to ask for a longer MSRV than 2 months? Debian is at 1.63, Guix is at 1.68. I don't think supporting a Rust version for at least a year is unreasonable to ask..

---

_Comment by @epage on 2023-08-24 17:25_

Our stated policy has been "We will support the last two minor Rust releases".  We have occasionally stretched it to be older as an experiment to see how well that works.  However, the value of being able to access the `IsTerminal` trait and avoid the entire dependency tree of `is-terminal` was of high enough value for us to upgrade.  I suspect we'll do so again when 1.76 is released as rust-lang/rust#111544 will be available within our MSRV.

Clap's policy is fairly common in the ecosystem.  Personally, tracking MSRV policy to something like Debian is a non-starter.  1.63 sounds new-ish now (though it misses workspace inheritance which is a big help for maintainers) but what will that look like in a year or two?  Until recently, Debian was using 1.41 which was pretty ancient in Rust terms by the time they updated.

Note that the cargo has changed its guidance on committing lockfiles.  The new guidance is [do what works for your project with committing as the default choice](https://doc.rust-lang.org/nightly/cargo/faq.html#why-have-cargolock-in-version-control) (expect a rust-lang blog post soon talking about this).  I highly encourage anyone maintaining an MSRV to commit their lockfile (it was one of the motivating reasons for me to push that policy change).

I also hope that soon I'll be able to get back to making cargo's resolver MSRV aware.

---

_Referenced in [clap-rs/clap#5087](../../clap-rs/clap/issues/5087.md) on 2023-08-24 23:40_

---

_Comment by @stevenroose on 2023-08-30 01:56_

2 months is incredibly short, though.. I can see that a large project might have a larger incentive than some smaller projects to upgrade (I still maintain 1.29 in some projects and generally we support 1.48 now), but even a really tempting new shiny thing can wait more than 2 months, right?

But yeah you're right, lockfiles help in almost all such cases. Having msrv taken into account in the resolver would make it so much easier. If you can get that to work in cargo, you'd be _Rust Hero Number One_, granted.

---

_Comment by @epage on 2023-08-30 02:12_

MSRV-aware resolver is now in nightly under `-Zmsrv-policy`. This was done by taking a lot of shortcuts so it still has a long road ahead.

---

_Comment by @dhardy on 2023-12-30 12:13_

... and I had to look this far to find the actual MSRV policy (and related discussion).

Please make it clearer (e.g. put it in the README). Going by the broken link above, I'm guessing it was removed at some point?

---

_Comment by @epage on 2023-12-30 12:51_

We've tried to centralize our documentation on docs.rs. For the MSRV policy, see https://docs.rs/clap/latest/clap/#aspirations

---

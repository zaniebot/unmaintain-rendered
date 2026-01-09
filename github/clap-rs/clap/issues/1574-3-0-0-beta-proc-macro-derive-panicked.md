---
number: 1574
title: "3.0.0 beta: proc-macro derive panicked"
type: issue
state: closed
author: Walther
labels: []
assignees: []
created_at: 2019-10-17T22:03:36Z
updated_at: 2020-02-01T11:29:51Z
url: https://github.com/clap-rs/clap/issues/1574
synced_at: 2026-01-07T13:12:19-06:00
---

# 3.0.0 beta: proc-macro derive panicked

---

_Issue opened by @Walther on 2019-10-17 22:03_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.38.0 (625451e37 2019-09-23)`

### Affected Version of clap

```
[[package]]
name = "clap"
version = "3.0.0-beta.1"
source = "git+https://github.com/clap-rs/clap#b8819130de876c4abbd451c34df89161a98d23ef"
```

### Expected Behavior Summary

`#[derive(Clap, Debug)]` and `#[clap()]` macros should work

### Actual Behavior Summary

`proc-macro derive panicked`. Screenshots help here for showing how it varies when commenting out macros; it seems to fail in different ones too.

<img width="841" alt="Screen Shot 2019-10-18 at 00 56 57" src="https://user-images.githubusercontent.com/2943750/67050944-6ec43a00-f142-11e9-8e26-c81307aada0d.png">
<img width="828" alt="Screen Shot 2019-10-18 at 00 57 07" src="https://user-images.githubusercontent.com/2943750/67050945-6ec43a00-f142-11e9-9815-1720cbf4636b.png">


### Steps to Reproduce the issue

1. Use `clap` from git master
2. Follow the instructions in readme; use `#[derive(Clap, Debug)]` and `#[clap()]` macros
3. Observe compile errors from `rustc`

### Sample Code or Link to Sample Code

Tiny pet project pull request here for upgrading to `clap` 3.0.0 here, with a very minimal reproduction of the issue: https://github.com/Walther/wordcrab/pull/1

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>

cargo build --verbose
       Fresh unicode-xid v0.1.0
       Fresh unicode-width v0.1.6
       Fresh strsim v0.9.2
       Fresh indexmap v1.2.0
       Fresh ansi_term v0.11.0
       Fresh vec_map v0.8.1
       Fresh itoa v0.4.4
       Fresh textwrap v0.11.0
       Fresh proc-macro2 v0.4.30
       Fresh libc v0.2.64
       Fresh bitflags v1.2.1
       Fresh ryu v1.0.2
       Fresh serde v1.0.101
       Fresh quote v0.6.13
       Fresh atty v0.2.13
       Fresh serde_json v1.0.41
       Fresh syn v0.15.44
       Fresh clap_derive v0.3.0 (https://github.com/clap-rs/clap_derive#4a46684b)
       Fresh clap v3.0.0-beta.1 (https://github.com/clap-rs/clap#b8819130)
   Compiling wordcrab v0.2.1 (/Users/veeti/git/wordcrab)
     Running `rustc --edition=2018 --crate-name wordcrab src/main.rs --color always --crate-type bin --emit=dep-info,link -C debuginfo=2 -C metadata=94f2349a0e5cbdc7 -C extra-filename=-94f2349a0e5cbdc7 --out-dir /Users/veeti/git/wordcrab/target/debug/deps -C incremental=/Users/veeti/git/wordcrab/target/debug/incremental -L dependency=/Users/veeti/git/wordcrab/target/debug/deps --extern clap=/Users/veeti/git/wordcrab/target/debug/deps/libclap-5caa4c477358f1f8.rlib --extern serde=/Users/veeti/git/wordcrab/target/debug/deps/libserde-e483c8ba7a1e6576.rlib --extern serde_json=/Users/veeti/git/wordcrab/target/debug/deps/libserde_json-098c2405408937ce.rlib`
error: proc-macro derive panicked
 --> src/main.rs:9:10
  |
9 | #[derive(Clap, Debug)]
  |          ^^^^
  |
  = help: message: unsupported option: short

error: aborting due to previous error

error: Could not compile `wordcrab`.

Caused by:
  process didn't exit successfully: `rustc --edition=2018 --crate-name wordcrab src/main.rs --color always --crate-type bin --emit=dep-info,link -C debuginfo=2 -C metadata=94f2349a0e5cbdc7 -C extra-filename=-94f2349a0e5cbdc7 --out-dir /Users/veeti/git/wordcrab/target/debug/deps -C incremental=/Users/veeti/git/wordcrab/target/debug/incremental -L dependency=/Users/veeti/git/wordcrab/target/debug/deps --extern clap=/Users/veeti/git/wordcrab/target/debug/deps/libclap-5caa4c477358f1f8.rlib --extern serde=/Users/veeti/git/wordcrab/target/debug/deps/libserde-e483c8ba7a1e6576.rlib --extern serde_json=/Users/veeti/git/wordcrab/target/debug/deps/libserde_json-098c2405408937ce.rlib` (exit code: 1)

</code>
</pre>
</details>


---

_Comment by @Walther on 2019-10-17 22:23_

On a quick search through the codebase, I don't think I found any tests that are using either of the macros? Only mentions of the macros are in the Readme:
<img width="960" alt="Screen Shot 2019-10-18 at 01 22 53" src="https://user-images.githubusercontent.com/2943750/67052097-d760e600-f145-11e9-99b4-4dc63191da48.png">


---

_Comment by @Dylan-DPC-zz on 2019-10-17 22:25_

The macros are declared in clap_derive crate so you can check there 

---

_Comment by @Walther on 2019-10-17 22:26_

Ah, this issue would probably belong into that repo then 🤔  Sorry about that!

---

_Comment by @Walther on 2019-10-17 22:45_

Found some more leads: I was trying to use this deduced-form, from structopt documentation: <https://docs.rs/structopt/0.3.3/structopt/>
```
// short and long flags (-d, --debug) will be deduced from the field's name
    #[structopt(short, long)]
```
whereas `clap_derive` tests and Readme only uses the explicit form `short = "x"`. Converting all of those uses is fairly easy, not sure if clap 3 intends to offer the deduced short form from structopt.

---

Another lead: through reduction,I noticed `possible_values = &["text", "json"]` within a `#[clap()` macro also makes it fail. Ditto for `global_settings = &[AppSettings::ColoredHelp]`.

Removing those two and converting all the short,long uses into explicit form makes it work again.

Pushed an additional commit to my example PR to show the required changes. https://github.com/Walther/wordcrab/pull/1/commits/7a676569e9da447e7ea734062cf6c12172e5fe43

---

_Comment by @CreepySkeleton on 2019-10-18 15:15_

@Walther please, don't look at `structopt`'s docs when dealing with `clap_derive`. `clap_derive` is essentially a year-old `structopt`, more importantly,  in utilizes `structopt v0.2.x`. `structopt v0.3` (current) is not backward compatible with `v0.2`.

@Dylan-DPC what are the plan for `clap_derive` release? Are you going to continue using `v0.2`? I this case we will end up with situation when we have an "official" derive (`clap_derive`) and "third-party" derive (`structopt`), worst part, incompatible. I would really love to evade this situation. Any thoughts?

---

_Comment by @Dylan-DPC-zz on 2019-10-18 17:49_

@CreepySkeleton 
> Are you going to continue using v0.2?
I didn't get this part. 

Also clap-derive is just coincidentally compatible with structopt. This isn't a guarantee and both could diverge at any time in the future. 

---

_Comment by @CreepySkeleton on 2019-10-18 18:40_

> I didn't get this part.

@Dylan-DPC I mean, `clap-derive` was started as a `structopt 0.2`'s fork, even now its codebase is very very similar to `structopt`'s. I thought (and still think) the intention was to make a "better" structopt, more closely tied to `clap` and offer some additional features. Even in the readme you mention that derive_clap is essentially an import and, eventually, replacement for structopt, but for now this is simply not true. 

`structopt` has released a new minor version, which is, in my opinion, a big improvement, it offers a number of additional features `clap-derive` lacks (not to mention bug fixes). Also, community is moving to `0.3` version, see the [downloads statistic](https://crates.io/crates/structopt), so when `clap-derive` will be eventually released, community is going to have a hard time adapting their codebase to `clap-derive`. Or, more likely, they'll just stay with `structopt`, because `clap-derive` doesn't really offer anything that one can't have with `structopt`. 

That said, I propose to meld the **current** `structopt` into `clap_derive` thus having new features/bugfixes in the clap itself and make them more-or-less compatible so people could migrate with not much effort. 

> Also clap-derive is just coincidentally compatible with structopt. This isn't a guarantee and both could diverge at any time in the future.

Quoting the readme:
"Since structopt already used clap under the hood, the transition was nearly painless, and is 100% feature compatible."

Who's to believe? :smile: 

---

_Comment by @Walther on 2019-10-19 11:01_

What would be the ideal end result? Can I help in getting towards that direction? 🙂

Additionally, is is possible to somehow improve the error messages of the proc macro failures, or is there some technical limitation in how they work?

I understand now that fundamentally, this issue was caused by trying to use structopt 0.3 features in clap 3.0 beta, and some of the features and uses not aligning. Took a bit of figuring out though, as the proc macros did not provide in-editor hints about existing properties or specific error messages.

---

_Comment by @CreepySkeleton on 2019-10-19 11:09_

> Additionally, is is possible to somehow improve the error messages of the proc macro failures, or is there some technical limitation in how they work?

This is done in [`structopt v0.3`](https://github.com/TeXitoi/structopt/blob/master/CHANGELOG.md#significantly-improve-error-reporting-by-creepyskeleton-225) as one of the new features I was talking about. That's why I'm trying to encourage people to move for v0.3 - most of the asked improvements are already done in the new structopt.

> Took a bit of figuring out though, as the proc macros did not provide in-editor hints about existing properties or specific error messages.

If you think that some error message in `structopt` isn't clear enough or points to a wrong location - please [file an issue](https://github.com/TeXitoi/structopt/issues).

> What would be the ideal end result?

IMHO: the ideal result would be merging the latest `structopt` into `clap_derive` and deprecating `structopt` entirely.

---

_Referenced in [clap-rs/clap#1584](../../clap-rs/clap/issues/1584.md) on 2019-10-25 11:39_

---

_Comment by @pksunkara on 2020-01-18 15:43_

This has been fixed with the recent import of structopt into clap_derive. But it still doesn't solve the issue of structopt vs clap. I propose that either @CreepySkeleton or @TeXitoi make a plan to merge structopt into clap_derive and combine the efforts.

Does structopt support clap v3? Because clap_derive does

---

_Comment by @TeXitoi on 2020-01-18 15:58_


Structopt is for clap 2, clap-derive for clap 3

18 janv. 2020 16:43:20 Pavan Kumar Sunkara <notifications@github.com>:

> 
> This has been fixed with the recent import of structopt into clap_derive. But it still doesn't solve the issue of structopt vs clap. I propose that either @CreepySkeleton [https://github.com/CreepySkeleton] or @TeXitoi [https://github.com/TeXitoi] make a plan to merge structopt into clap_derive and combine the efforts.
> 
> Does structopt support clap v3? Because clap_derive does
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub [https://github.com/clap-rs/clap/issues/1574?email_source=notifications&email_token=ABME3OUDQLGAUSJ6BUK4JHLQ6MPRNA5CNFSM4JB77IGKYY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGOEJJ3BIA#issuecomment-575910048] , or unsubscribe [https://github.com/notifications/unsubscribe-auth/ABME3OV4CN7CX4LPG2L45L3Q6MPRNANCNFSM4JB77IGA] . [https://github.com/notifications/beacon/ABME3OSXA65EVP3CB6KKSDLQ6MPRNA5CNFSM4JB77IGKYY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGOEJJ3BIA.gif]
> 



---

_Comment by @CreepySkeleton on 2020-01-18 17:12_

### "`clap_derive` release" plan

The first thing we need is to import all the "not yet imported" commits, including the two pending PRs.

Then we need to decide if we want to keep `no_version`, I believe it must be done before the "beta" release. I highly encourage you **not to keep it**. It doesn't work well, we've already got few bugs because of it, see [this](https://github.com/TeXitoi/structopt/issues/243), [that](https://github.com/TeXitoi/structopt/issues/242), and [those](https://github.com/TeXitoi/structopt/issues/324) [two](https://github.com/TeXitoi/structopt/issues/283) bug reports. All of them were fixed by *dirty hacks* in the code. Let's just replace it with explicit `#[clap(version)]` to be consistent with `#[clap(author, about)]`.

Once this is resolved, we're making the "beta" release (including on crates.io) to gather some early feedback (we may need some sort of announce for that). All the problems outlined [there](https://rust-lang.zulipchat.com/#narrow/stream/220302-wg-cli/topic/clap-vs-structopt) will be resolved based on this feedback (except for the refactoring, we need it most certainly and desperately but I admit it can be postponed for a bit).

@Dylan-DPC any thoughts?

### "After `clap_derive` release" plan

@TeXitoi and I had a short conversation about it and we came up with the following:

Once `clap_derive` is released, `structopt` will become "softly deprecated". This means that both @TeXitoi and I will be continue maintaining it in "passively maintained" mode. 

We will be merging bugfixes and the like but `structopt` will not receive future evolution. We will not be merging feature requests and neither we will work on our own ones. Such issues/PRs will be transferred to the `clap_derive` repository (or `clap`, whatever).

I may be interested in backporting certain features from `clap_derive` to `structopt` in the future on personal basis if the feature in question is a commonly requested one; but this won't be my priority and I don't guarantee anything.

I would like to empathize here that neither @TeXitoi nor I are interested in providing support for `clap v3` in structopt, `structopt` will stay on `clap v2.33`. We will not backport such requests, let's move forward, not backward.

@TeXitoi does it sound good to you?

---

_Comment by @TeXitoi on 2020-01-18 17:40_

Yup, I agree. I also recommend to remove the no_version thing. Also, There might have other breaking changes that might be wanted, that's the time to talk about that before the beta, or it will be too late.

---

_Comment by @CreepySkeleton on 2020-01-18 17:49_

Hmm, I thought "beta" release are not the "final one", and the breaking changes are possible after it.

If not, we will also need to come to an agreement on "multiple traits" thing and `Parse` trait thing.

---

_Comment by @pksunkara on 2020-01-18 17:50_

I am not a contributor to structopt but the plan sounds good to me.

Regarding breaking, I think as long as we haven't released the major version, it should be okay breaking things between pre releases. At least that's what I understand from Denver. Please correct me if I'm wrong.

---

_Comment by @pksunkara on 2020-01-18 17:51_

I meant semver. Not Denver. :smile:

---

_Comment by @CreepySkeleton on 2020-01-18 17:54_

Citing semver  (emphasis mine)

> A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers immediately following the patch version. Identifiers MUST comprise only ASCII alphanumerics and hyphen [0-9A-Za-z-]. Identifiers MUST NOT be empty. Numeric identifiers MUST NOT include leading zeroes. Pre-release versions have a lower precedence than the associated normal version. **A pre-release version indicates that the version is unstable and might not satisfy the intended compatibility requirements as denoted by its associated normal version**. Examples: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92.

---

_Comment by @TeXitoi on 2020-01-18 17:54_

Even if that's true, breaking changes on beta should be more about fixing from feedback on the beta than continuing breaking because we're not yet fixed on what we think is good.

---

_Comment by @CreepySkeleton on 2020-01-18 17:55_

@TeXitoi good point 👍 Let's settle this "traits" things up today.

---

_Comment by @CreepySkeleton on 2020-01-18 17:58_

Or tomorrow 😄 

---

_Comment by @Dylan-DPC-zz on 2020-01-18 18:35_

Okay a few things:

First, let's discuss release policies separately and not here. 

> Let's just replace it with explicit #[clap(version)] to be consistent with #[clap(author, about)].

@TeXitoi  agree with this

> Hmm, I thought "beta" release are not the "final one", and the breaking changes are possible after it.

Yes generally the design and the API remains the same and the (breaking) changes are for bug fixes and other things that were overlook rather than adding new features. 
 

---

_Comment by @CreepySkeleton on 2020-02-01 11:29_

Fixed by `structopt`'s partial import.

---

_Closed by @CreepySkeleton on 2020-02-01 11:29_

---

_Referenced in [mozilla/grcov#648](../../mozilla/grcov/pulls/648.md) on 2021-07-22 14:13_

---

_Referenced in [databendlabs/openraft#116](../../databendlabs/openraft/pulls/116.md) on 2022-01-16 15:05_

---

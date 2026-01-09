---
number: 1037
title: Tracking Issue for 3.x
type: issue
state: closed
author: kbknapp
labels:
  - E-hard
assignees: []
created_at: 2017-08-21T22:18:23Z
updated_at: 2021-10-13T11:24:24Z
url: https://github.com/clap-rs/clap/issues/1037
synced_at: 2026-01-07T13:12:19-06:00
---

# Tracking Issue for 3.x

---

_Issue opened by @kbknapp on 2017-08-21 22:18_

**UPDATED**: [3.0 Milestone](https://github.com/clap-rs/clap/milestone/76)

---

This an internal tracking issue for what I'd like to get done in order to release 3.x. Granted not all of these things must happen in order for a release to happen, but it gives a good idea of what is left to do.

Also, this isn't a fully comprehensive list, or may not even make sense...it's mainly meant as an internal reminder to me.

Once 3.x is out, I should be able to rip through implementing all the feature requests that are piling up

All work is taking place on v3-master, wip work is on v3-dev

- [ ] = not started
- [x] = complete

## 3.x-beta Blockers

- [x] Deprecations
   - [x]  `App`
   - [x] (Re)Move inherently global `AppSettings` and prefer `App::global_setting`
   - [x]  `Arg`
      - [x] `Arg::from_usage` -> `Arg::from(&str)` 
      - [x]  Validator signatures
         - [x] Make validators more flexible #1243 
         - [x] #848 (`validator_os` should return `String` instead of `OsString`)
         - [x] #1165 (`validator` takes `String` but `validator_os` takes `&OsStr`)
   - [x] `SubCommand` -> `App` (stop the facade)
- [x]  `App` additions
   - [x] `App::get_matches_mut` (#950 Lib Blitz naming consistency)
   - [x] `App::mut_arg` (#458)
   - [x] `App::unset_global_setting` (#1183)
- [x] `Arg` additions
  - [x] `Arg::new`
  - [x] `Arg::settings`
  - [x] `Arg::unset_settings`
  - [x] `impl From<&str> for Arg`
- [x] Restructure mods
- [x] Update `yaml_rust` to 0.4
- [x]  Custom sections (relates to #805 )
- [x] Redo Arg/App internals (True Builders)
  - [x] App
  - [x] ArgGroup
  - [x] Arg
  - [x] Make tests pass
- [x]  "Back port" 2.x fixes and features
   - [x]  #976 
      - [x] self overrides
      - [x] all args override self setting
   - [x] #1158 (`Arg::requires_all` not displaying usage correctly)
   - [x] #1155 
   - [x] #1160
   - [x] #1161 (Nesting `--`)
   - [x] #1179 
   - [x] #1189 (Short help in PowerShell tool-tips)
   - [x] #1190 (WASM)
   - [x] #1191 (`Arg::raw`)
   - [x] #1192 (`Cargo.toml` `code-gen-units = 1` in release mode)
   - [x] #1195 (Add `wrap_help` feature to README)
   - [x] #1196 (`Indices`) 
   - [x] #1199 (Update `ansi_term` to 0.11)
   - [x] #1200 (Fix typos in docs)
   - [x] #1203 (Skipable Positional Args)
   - [x] #1209 (typo in docs)
   - [x] #1214 (fish shell fix)
   - [x] #1215 (disable suggestions with `AllowExternalSubcommands`)
   - [x] #1235 (hide arg from short/long help)
   - [x] #1238 (support shorthand syntax for `required(true)` with macros)
   - [x] #1283 (#897 always show long about for subcommands main help message)
   - [x] #1298 (exactly match subcommand when using `InferSubcommands`)
- [x] [clap_derive](https://github.com/clap-rs/clap_derive) crate
  - [x]  Pull in [`structopt`](https://github.com/TeXitoi/structopt) 
  - [x] `ArgEnum`
  - [x] `IntoApp`
  - [x] `FromArgMatches`
  - [x] `Clap`
- [x] Remove `usage` from `ArgMatches`
- [x] bump `term_size` to 1.0
- [x] Change default help template (`ARGS` should be first in the sections but last in usage)
- [x] refactor completion script generation into [`clap_generate`](https://github.com/clap-rs/clap_generate)  (#951)

## 3.x-rc Blockers

- [x] Derive all common traits (#952)
- [x] Remove all deprecations
- [x] Update all examples/tests to new calls
  - [x] Remove deprecations
  - [x] fix warnings
- [ ] Fix App's bin_name vs name vs usage delima
- [ ] Lib Blitz Overhaul (#950)
   - [ ] Update docs
- [ ] v3-rc release
   - [ ] doc scrub (spell check)
   - [x] clippy run
   - [ ] Blog post about release schedule
   - [x] rustfmt Run

## 3.0 Blockers

- [ ] 3.0 Release
   - [ ] Double check all docs
   - [ ] Double check all examples
   - [ ] Double Check readme
   - [ ] Double check examples on clap.rs
   - [ ] Changelog
   - [x] Clippy Run
   - [x] Rustfmt Run
   - [ ] Spell Check Docs
   - [ ] Blog Post

## Items being punted for now (priority after 3.0 release, or if time permits before)

* Get usage lazily
* Lazy propagation of subcommands and globals
* New Key type for Arg|App|ArgGroup (#1104) 
* Decide on lifetime issues (#1041) 
* Color Redux (#836 #942)
* Serde (#853 #965 #918 #747) (`serde_yaml` can't parse into borrowed structs yet, see https://github.com/dtolnay/serde-yaml/issues/94)
  * yaml tests
  * `Arg::from_yaml` -> `serde`
  * Prep for `App` for `serde`
* Cleanup Debug Output
* Generate manpages in `clap_generate` (#552)
* Global Help Templates (#1184 )
* Auto generate certain flags/subcommands
   * negation flags (#815)
   * completions subcommand (#810)
   * completions option (#810)
* Filters (similar to validators)
* Maps (similar to validators)
* Actions (similar to validators)
* `App::argv` (#748)
* `App::env_argv` (#748)
* manpage generation (#552 #555)
* Complete dynamic values (#568 #1232)
* Decide on in tree crate or not (will probably be determined by following `clap_derive`'s move
* move `clap-test.rs` to into the src
  * this would incur additional deps that are currently dev-deps only
* clap_macros crate
  * Not sure it's worth it...this one is TBD
* `impl From<App> for ArgMatches`
  * Additional information is required (`env::args_os` etc.)
* `impl TryFrom<App> for ArgMatches`
  * Same as `From<App>`
*  `AppSettings` -> direct use of bitflags (no more facade)
* Docopt parser (it's a pretty major under taking and I want 3.x to be released)


---

_Label `W: 3.x` added by @kbknapp on 2017-08-21 22:18_

---

_Added to milestone `3.0` by @kbknapp on 2017-08-21 22:18_

---

_Referenced in [clap-rs/clap#1154](../../clap-rs/clap/pulls/1154.md) on 2018-01-26 04:15_

---

_Label `P1: urgent` added by @kbknapp on 2018-01-26 04:22_

---

_Label `D: hard` added by @kbknapp on 2018-01-26 04:22_

---

_Assigned to @kbknapp by @kbknapp on 2018-01-26 04:22_

---

_Referenced in [clap-rs/clap#1155](../../clap-rs/clap/issues/1155.md) on 2018-01-31 01:35_

---

_Referenced in [clap-rs/clap#1165](../../clap-rs/clap/issues/1165.md) on 2018-02-05 15:23_

---

_Referenced in [rust-cli/team#1](../../rust-cli/team/issues/1.md) on 2018-02-20 18:21_

---

_Referenced in [rust-cli/team#8](../../rust-cli/team/issues/8.md) on 2018-02-20 19:19_

---

_Referenced in [rust-cli/team#14](../../rust-cli/team/issues/14.md) on 2018-03-01 16:10_

---

_Referenced in [rust-cli/team#23](../../rust-cli/team/issues/23.md) on 2018-03-22 13:49_

---

_Removed from milestone `3.0` by @kbknapp on 2018-07-22 00:44_

---

_Referenced in [rust-lang/crates.io#1504](../../rust-lang/crates.io/issues/1504.md) on 2018-09-28 09:27_

---

_Referenced in [clap-rs/clap#1559](../../clap-rs/clap/pulls/1559.md) on 2019-10-02 13:29_

---

_Referenced in [clap-rs/clap#1606](../../clap-rs/clap/issues/1606.md) on 2019-12-10 19:52_

---

_Added to milestone `3.0` by @pksunkara on 2020-02-03 10:44_

---

_Comment by @pickfire on 2020-03-29 11:39_

Do we still need `Arg::new()`, didn't we remove it last time?

---

_Comment by @pksunkara on 2020-03-29 15:45_

I am not exactly sure about it since I wasn't involved in the project back then. Do you have a link to the commit or PR which removed `Arg::new`?

---

_Comment by @CreepySkeleton on 2020-03-29 19:34_

I guess I did it. It didn't have any docs and was something like:
```rust
fn new<T: Into<Key>>(name: T) {}
```
The idea was apparently to allow using anything that's `Into<Key>` (clap's synonym for hashing), but I really doubt this could be useful. For what?

---

_Comment by @pksunkara on 2020-03-29 21:42_

Isn't `Arg::new` the thing used for building args?

---

_Comment by @pksunkara on 2020-03-29 21:46_

Can you link the commit which removed it? I tried to find it but couldn't.

---

_Comment by @pickfire on 2020-03-30 02:48_

https://github.com/clap-rs/clap/commit/2b3d3fd901e117842891e5794d36ca13b9401c94

CHANGELOG https://github.com/clap-rs/clap/blob/6422930a1864fdfd32b15f01e76bc0aa428cdd52/CHANGELOG.md#breaking-changes-2

I cannot find the original commit that removes `Arg::new`.

---

_Comment by @CreepySkeleton on 2020-03-30 08:44_

> Isn't `Arg::new` the thing used for building args?

Args are being build via `Arg::with_name` and `Arg::from` (`Arg::from_usage` and `Arg::clone` in 2.x). 

That's funny: I haven't removed it after all
https://github.com/clap-rs/clap/blob/37889c661134e8286102f7d2ab3267965d010403/src/build/arg/mod.rs#L101-L109

I clearly remember that I was *thinking* about removing it as useless, but apparently I didn't make my mind up for it.

---

_Comment by @pickfire on 2020-03-30 16:01_

Yeah, it was only removed from the docs. That's why we cannot find the commits.

---

_Referenced in [alacritty/alacritty#3617](../../alacritty/alacritty/issues/3617.md) on 2020-04-20 07:26_

---

_Referenced in [dtolnay/cxx#271](../../dtolnay/cxx/pulls/271.md) on 2020-08-30 20:55_

---

_Comment by @intgr on 2020-09-15 19:25_

Any chance for another beta release, even if all "beta blockers" aren't fixed yet? I've been waiting to take advantage of #1793 in my pet project. :)

---

_Comment by @CreepySkeleton on 2020-09-16 07:22_

I have no problems with that. @pksunkara what do you think?

---

_Comment by @pksunkara on 2020-09-16 07:29_

I would prefer to finish off fixing the `multiple` issue and then doing the beta.

---

_Comment by @CreepySkeleton on 2020-09-16 12:15_

This `multiple` thing is not going to happen this week. My free time is severely limited at the moment, and the first thing I'm about to start working on is globals (I promised it back in May, dammit) and empty values (almost finished and been brewing for two months).

The only reason not to release a beta I can think of is - if we wanted to discourage people form depending on unfinished, work-in-progress state of clap, we could avoid any beta releases altogether and require people to use git dependency. That ship has sailed - we have a beta released already, and we've been planning to release several betas all along. 

Well, not only one. Another reason might be that we wanted to present a "production ready-ish" product, complete (kinda). That would be nice indeed, PR, wow-effect and all that, but our turtle speed of development essentially means that either we do a series of intermediate releases, or there will be no release at all in the near future. It's me at fault more than anybody else, I'm sorry about it, but life takes precedence, and I can't/won't spend much of my time on working on clap right now. 

Then again, there's @intgr with their contribution. I think they are liable to expect their contribution to become available on crates.io within some reasonable time period. By saying "Let's postpone the release for the sake of having \<feature> in the next release, and don't mind that nobody's actively working on it ATM, no deadlines have been set, and it's not immediately clear what to do with it anyway", we're only discouraging future contributors from putting more work in clap.

Summarizing all that: given the lack of actual commitment from core contributors for various reasons (which is my polite phrasing for "I am a lazy butt, sorry"), slow pace of development overall, and an explicit request to make the ongoing work available on cartes.io from several people, I don't think there's much point in postponing beta releases on and on.

(And I think the release is going to happen [one way](https://crates.io/crates/clap-v3) or [another](https://crates.io/crates/clap_derive-v3) whether we want it or not.)

---

_Comment by @pksunkara on 2020-09-16 12:31_

Okay I will do the beta release tonight.

---

_Comment by @Dylan-DPC-zz on 2020-09-16 12:33_

Those forks were 6 months back and they might have been plenty of unreleased ones which we may never heard off and this is a common thing in the open source world for people to maintain open source forks in private repositories / company domains because it's more "reliable". 

I don't mind having beta releases, but you need to ask the question at what cost? if it's just to test random unrelated fixes that it just causes more churn to have the releases & ensure everything is proper and working and not regressing between betas. People are not discouraged to contribute whether we have betas or not, people are discouraged to contribute when they see a lack of transparency and they didn't know the state of something because things get closed because "they are fixed on master" or "because their feature is not being seen as acceptable by maintainers or it is worked on or recommended to use another way. 

There is no "turtle speed of development" (not sure that's even a term because turtles are known to be pretty decent paced irl :P but i understood what you meant) but there's a "effective turtle speed of development". There are too many changes being happening regularly and being merged quickly that it creates enough of churn for people including maintainers to keep track of what's happening. Many times I feel like answering questions or reviewing prs but then i need to check and see what's the reason the changes are made and track as it's refactored multiple times and it's merged quickly enough that there's no point of putting that much of effort. 

I know most of the (other) contributors haven't been active enough but that's life and clap seems to be a "happy stage"  that it may not warrant rushing things. I (and i'm sure the other maintainers) respect your contributions to clap, but if you feel this is just taking up your time and you are not sure of the path ahead, you are free to step down as a maintainer and we won't hold you responsible for anything.  

---

_Comment by @pksunkara on 2020-09-27 20:50_

@kbknapp Can you explain the below a bit?

> (Re)Move inherently global `AppSettings` and prefer `App::global_setting`

It is the only thing I don't understand from the main post.

---

_Comment by @kbknapp on 2020-09-28 00:41_

There are some `AppSettings` which don't make sense in a non global context, meaning using for only a single subcommand doesn't make sense. `GlobalVersion` is one example, but there are several. These settings were designed with the whole application in mind. The intent behind that phrase you quoted was to move away from `AppSettings` which only make sense in a global context and instead use variations which only apply to a single command, but allow the user to select `App::global_setting` if they preferred applying the settings to the whole application.

The reasoning behind this is so that we didn't have some settings magically affecting the whole application and others only a single command which has caused confusion in the past.

---

_Referenced in [Aphosis/gmux#7](../../Aphosis/gmux/issues/7.md) on 2020-10-27 12:56_

---

_Referenced in [uutils/coreutils#2138](../../uutils/coreutils/pulls/2138.md) on 2021-05-03 08:38_

---

_Referenced in [iqlusioninc/abscissa#298](../../iqlusioninc/abscissa/issues/298.md) on 2021-05-12 01:05_

---

_Referenced in [denoland/deno#6237](../../denoland/deno/issues/6237.md) on 2021-06-15 23:59_

---

_Referenced in [clap-rs/clap#2681](../../clap-rs/clap/pulls/2681.md) on 2021-08-11 21:45_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Comment by @sylvestre on 2021-08-22 13:24_

Any chance clap 3 could be released in the current state? Even if this isn't perfect, it is already a significant improvement over the version 2.


---

_Comment by @epage on 2021-08-23 14:06_

A conversation about narrowing the scope for remaining 3.0 work is happening in https://github.com/clap-rs/clap/discussions/2616

---

_Referenced in [clap-rs/clap#2717](../../clap-rs/clap/issues/2717.md) on 2021-09-23 11:27_

---

_Comment by @epage on 2021-10-13 11:24_

https://github.com/clap-rs/clap/issues/2869 should cover the rest of this

---

_Closed by @epage on 2021-10-13 11:24_

---

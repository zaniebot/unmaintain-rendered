---
number: 2167
title: Panic on subcommand without -h
type: issue
state: closed
author: Absolucy
labels:
  - C-bug
  - A-derive
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-10-11T03:05:32Z
updated_at: 2021-02-28T12:37:31Z
url: https://github.com/clap-rs/clap/issues/2167
synced_at: 2026-01-10T01:27:13Z
---

# Panic on subcommand without -h

---

_Issue opened by @Absolucy on 2020-10-11 03:05_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
#[derive(Debug, Clap, Clone)]
#[clap(author, about, version)]
pub struct FurnacaOpts {
	#[clap(short, long)]
	pub endpoint: Option<String>,
	#[clap(subcommand)]
	pub subcmd: Subcommand,
}

#[derive(Debug, Clap, Clone)]
pub enum Subcommand {
	/// Upload a resource to the Cinderferno Anthracite Controller.
	#[clap(author, version)]
	Upload(UploadSubcommand),
	/// Manage and view processes in the Cinderferno Anthracite Controller.
	#[clap(author, version)]
	Task(TaskSubcommand),
	/// Manage authentication for the Cinderferno Anthracite Controller.
	#[clap(author, version)]
	Auth(AuthSubcommand),
}

#[derive(Debug, Clap, Clone)]
pub enum AuthSubcommand {
	/// Log in to the given endpoint.
	#[clap(author, version)]
	Login,
	/// Register an account on the given endpoint.
	#[clap(author, version)]
	Register,
}

```

### Steps to reproduce the issue

1. Run `cargo run -- auth`
2. Panic

### Version

* **Rust**: `rustc 1.47.0 (18bf6b4f0 2020-10-07)`
* **Clap**: `3.0.0-beta.2`

### Actual Behavior Summary

Running `cargo run -- auth` causes a panic.  
`cargo run -- auth -h` works.

### Expected Behavior Summary

`cargo run -- auth` and `cargo run -- auth -h` should have identical behavior.
### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/furnaca auth`
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:furnaca
[            clap::build::app] 	App::_propagate:upload
[            clap::build::app] 	App::_propagate:arxd
[            clap::build::app] 	App::_propagate:arep
[            clap::build::app] 	App::_propagate:task
[            clap::build::app] 	App::_propagate:create
[            clap::build::app] 	App::_propagate:status
[            clap::build::app] 	App::_propagate:list
[            clap::build::app] 	App::_propagate:download
[            clap::build::app] 	App::_propagate:auth
[            clap::build::app] 	App::_propagate:login
[            clap::build::app] 	App::_propagate:register
[            clap::build::app] 	App::_derive_display_order:furnaca
[            clap::build::app] 	App::_derive_display_order:upload
[            clap::build::app] 	App::_derive_display_order:arxd
[            clap::build::app] 	App::_derive_display_order:arep
[            clap::build::app] 	App::_derive_display_order:task
[            clap::build::app] 	App::_derive_display_order:create
[            clap::build::app] 	App::_derive_display_order:status
[            clap::build::app] 	App::_derive_display_order:list
[            clap::build::app] 	App::_derive_display_order:download
[            clap::build::app] 	App::_derive_display_order:auth
[            clap::build::app] 	App::_derive_display_order:login
[            clap::build::app] 	App::_derive_display_order:register
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_create_help_and_version: Building help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:endpoint
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"auth"' ([97, 117, 116, 104])
[         clap::parse::parser] 	Parser::is_new_arg: "auth":NotFound
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: probably value
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser] 	Parser::possible_subcommand: arg="auth"
[         clap::parse::parser] 	Parser::get_matches_with: possible_sc=true, sc=Some("auth")
[         clap::parse::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[            clap::build::app] 	App::_propagate:furnaca
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:auth
[            clap::build::app] 	App::_propagate:login
[            clap::build::app] 	App::_propagate:register
[            clap::build::app] 	App::_derive_display_order:auth
[            clap::build::app] 	App::_derive_display_order:login
[            clap::build::app] 	App::_derive_display_order:register
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_create_help_and_version: Building help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::parse_subcommand: About to parse sc=auth
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:endpoint:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:endpoint: doesn't have default vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
[31mThe application panicked (crashed).[0m
Message:  [36mcalled `Option::unwrap()` on a `None` value[0m
Location: [35mfurnaca/src/opts.rs[0m:[35m29[0m

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ BACKTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  [96m                              ⋮ 15 frames hidden ⋮                              [0m
  16: [32mcore::panicking::panic[0m[90m::h4b079e3c35cc1b09[0m
      at [35m/rustc/18bf6b4f01a6feaf7259ba7cdae58031af1b7b39/library/core/src/panicking.rs[0m:[35m50[0m
  17: [32mcore::option::Option<T>::unwrap[0m[90m::hba745fdff1314f31[0m
      at [35m/home/aspen/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs[0m:[35m370[0m
  18: [91m<furnaca::opts::AuthSubcommand as clap::derive::FromArgMatches>::from_arg_matches[0m[90m::hc4d8be0609adf79b[0m
      at [35m/home/aspen/Documents/Code/Rust/Furnaca/furnaca/src/opts.rs[0m:[35m29[0m
  19: [91m<furnaca::opts::Subcommand as clap::derive::Subcommand>::from_subcommand[0m[90m::hea40e220140043fa[0m
      at [35m/home/aspen/Documents/Code/Rust/Furnaca/furnaca/src/opts.rs[0m:[35m16[0m
  20: [91m<furnaca::opts::FurnacaOpts as clap::derive::FromArgMatches>::from_arg_matches[0m[90m::hdf26c24c4630fd22[0m
      at [35m/home/aspen/Documents/Code/Rust/Furnaca/furnaca/src/opts.rs[0m:[35m12[0m
  21: [32mclap::derive::Clap::parse[0m[90m::h72870aa7e2a0b74a[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-beta.2/src/derive.rs[0m:[35m77[0m
  22: [91mfurnaca::main::{{closure}}[0m[90m::h799f475c739efc92[0m
      at [35m/home/aspen/Documents/Code/Rust/Furnaca/furnaca/src/main.rs[0m:[35m50[0m
  23: [91m<core::future::from_generator::GenFuture<T> as core::future::future::Future>::poll[0m[90m::h146979152c284007[0m
      at [35m/home/aspen/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/future/mod.rs[0m:[35m79[0m
  24: [32mtokio::runtime::enter::Enter::block_on::{{closure}}[0m[90m::h7edae76a9c0936c6[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/enter.rs[0m:[35m160[0m
  25: [32mtokio::coop::with_budget::{{closure}}[0m[90m::hb2a863926bd31cc0[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/coop.rs[0m:[35m127[0m
  26: [32mstd::thread::local::LocalKey<T>::try_with[0m[90m::h5553f8ad764e25de[0m
      at [35m/home/aspen/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs[0m:[35m265[0m
  27: [32mstd::thread::local::LocalKey<T>::with[0m[90m::hc63d5795c829ea80[0m
      at [35m/home/aspen/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs[0m:[35m241[0m
  28: [32mtokio::coop::with_budget[0m[90m::ha40946cffe9071a1[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/coop.rs[0m:[35m120[0m
  29: [32mtokio::coop::budget[0m[90m::h2ccdf72bd3270b20[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/coop.rs[0m:[35m96[0m
  30: [32mtokio::runtime::enter::Enter::block_on[0m[90m::h84012771fa9bb6da[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/enter.rs[0m:[35m160[0m
  31: [32mtokio::runtime::thread_pool::ThreadPool::block_on[0m[90m::hdab0eb972284a7ad[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/thread_pool/mod.rs[0m:[35m82[0m
  32: [32mtokio::runtime::Runtime::block_on::{{closure}}[0m[90m::hc28ccf4b366dc4b2[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/mod.rs[0m:[35m446[0m
  33: [32mtokio::runtime::context::enter[0m[90m::hf82380a188067b17[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/context.rs[0m:[35m72[0m
  34: [32mtokio::runtime::handle::Handle::enter[0m[90m::ha4d18a0b82f0811d[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/handle.rs[0m:[35m76[0m
  35: [32mtokio::runtime::Runtime::block_on[0m[90m::hf3a0cad5ac71806c[0m
      at [35m/home/aspen/.cargo/registry/src/github.com-1ecc6299db9ec823/tokio-0.2.22/src/runtime/mod.rs[0m:[35m441[0m
  36: [91mfurnaca::main[0m[90m::h076a92067d2fd5ca[0m
      at [35m/home/aspen/Documents/Code/Rust/Furnaca/furnaca/src/main.rs[0m:[35m24[0m
  37: [32mcore::ops::function::FnOnce::call_once[0m[90m::h2bd46879e54b4988[0m
      at [35m/home/aspen/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs[0m:[35m227[0m
  [96m                              ⋮ 11 frames hidden ⋮                              [0m

Run with COLORBT_SHOW_HIDDEN=1 environment variable to disable frame filtering.
Run with RUST_BACKTRACE=full to include source snippets.

</code>
</pre>
</details>


---

_Label `T: bug` added by @Absolucy on 2020-10-11 03:05_

---

_Renamed from "Panic on subcommand without `-h`" to "Panic on subcommand without -h" by @Absolucy on 2020-10-11 03:06_

---

_Comment by @pksunkara on 2020-10-11 06:43_

Minimal reproducible example:

```rust
use clap::Clap;

#[derive(Debug, Clap, Clone)]
pub enum Opt {
    Auth(AuthSubcommand),
}

#[derive(Debug, Clap, Clone)]
pub enum AuthSubcommand {
    Login,
}

fn main() {
    Opt::parse_from(&["prog", "auth"]);
}
```

---

_Label `C: derive macros` added by @pksunkara on 2020-10-11 06:44_

---

_Added to milestone `3.0` by @pksunkara on 2020-10-11 06:44_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-11 06:44_

---

_Label `:money_with_wings: $5` removed by @pksunkara on 2020-10-11 06:44_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-26 07:38_

---

_Comment by @jder on 2021-01-03 22:44_

I believe this is the same as https://github.com/clap-rs/clap/issues/2005. @CreepySkeleton, is there still an open question about the right behavior with sub-sub commands as enums? 

---

_Comment by @pksunkara on 2021-02-28 12:37_

Closing this as duplicate of #2005 

---

_Closed by @pksunkara on 2021-02-28 12:37_

---

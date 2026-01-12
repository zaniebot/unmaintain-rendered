```yaml
number: 2022
title: Obscure panic with heading and default value
type: issue
state: closed
author: CertainLach
labels:
  - C-bug
  - A-derive
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-07-18T14:40:03Z
updated_at: 2020-10-11T09:27:01Z
url: https://github.com/clap-rs/clap/issues/2022
synced_at: 2026-01-12T16:14:12Z
```

# Obscure panic with heading and default value

---

_@CertainLach_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

```rust
use clap::Clap;

fn main() {
	#[derive(Clap)]
	#[clap(help_heading = "HEAD")]
	struct Opt2 {
		#[clap(long, name = "size")]
		pub b: Option<usize>,
	}
	#[derive(Clap)]
	struct Opt {
		#[clap(flatten)]
		opt2: Opt2,

		#[clap(long, default_value = "32")]
		a: i32,
	}
	Opt::parse_from(&["test"]);
}
```

### Steps to reproduce the issue

1. Run `cargo run`

### Version

* **Rust**: rustc 1.46.0-nightly (346aec9b0 2020-07-11)
* **Clap**: master at 48d308a8ab9e96d4b21336e428feee420dbac51d

### Actual Behavior Summary

Code is panicking with
```
thread 'auto_default_value' panicked at 'called `Option::unwrap()` on a `None` value', clap_derive/tests/default_arg_value.rs:18:12
...
  13: core::panicking::panic
             at src/libcore/panicking.rs:50
  14: core::option::Option<T>::unwrap
             at /home/lach/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libcore/macros/mod.rs:10
  15: <default_arg_value::auto_default_value::Opt as clap::derive::FromArgMatches>::from_arg_matches
             at clap_derive/tests/default_arg_value.rs:18
  16: clap::derive::Clap::parse_from
             at /home/lach/jsonnet-rs/clap/src/derive.rs:29
  17: default_arg_value::auto_default_value
             at clap_derive/tests/default_arg_value.rs:20
  18: default_arg_value::auto_default_value::{{closure}}
             at clap_derive/tests/default_arg_value.rs:5
  19: core::ops::function::FnOnce::call_once
             at /home/lach/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libcore/ops/function.rs:233
  20: <alloc::boxed::Box<F> as core::ops::function::FnOnce<A>>::call_once
             at /rustc/346aec9b02f3c74f3fce97fd6bda24709d220e49/src/liballoc/boxed.rs:1081
  21: <std::panic::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/346aec9b02f3c74f3fce97fd6bda24709d220e49/src/libstd/panic.rs:318
  22: std::panicking::try::do_call
             at /rustc/346aec9b02f3c74f3fce97fd6bda24709d220e49/src/libstd/panicking.rs:348
  23: std::panicking::try
             at /rustc/346aec9b02f3c74f3fce97fd6bda24709d220e49/src/libstd/panicking.rs:325
  24: std::panic::catch_unwind
             at /rustc/346aec9b02f3c74f3fce97fd6bda24709d220e49/src/libstd/panic.rs:394
  25: test::run_test_in_process
             at src/libtest/lib.rs:541
  26: test::run_test::run_test_inner::{{closure}}
             at src/libtest/lib.rs:450
```

**If a project of yours is blocked due to this bug, please, mention it explicitly.**

Found this while extracting argument parsing from main crate in https://github.com/CertainLach/jsonnet-rs

### Expected Behavior Summary

It shouldn't panic, and successfully parse input

### Additional context

`default_arg_value.rs:18:12` points at `a: i32` line

With no `default_value` on `a: i32`, or  without `help_heading` on `Opt2` struct it works as expected

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
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_derive_display_order:clap_derive
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[            clap::build::app]  App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:size
[            clap::build::arg]  Arg::_debug_asserts:a
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::remove_overrides
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[]
</code>
</pre>
</details>


---

_Label `T: bug` added by @CertainLach on 2020-07-18 14:40_

---

_Added to milestone `3.0` by @pksunkara on 2020-07-19 10:50_

---

_Comment by @pksunkara on 2020-08-28 04:09_

Equivalent code is:

```rust
fn main() {
    let result = App::new("opt")
        .help_heading("HEAD")
        .arg(Arg::new("size").long("b"))
        .arg(Arg::new("a").long("a").default_value("32"))
        .get_matches();

    eprintln!("{:#?}", result);
}
```

That is not failing though. Looks like a bug in derive specifically.

---

_Label `C: derive macros` added by @pksunkara on 2020-08-28 04:10_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-09 16:38_

---

_Comment by @CertainLach on 2020-10-09 17:34_

It isn't derive failure, cargo expand shows this as only difference
```diff
--- broken      2020-10-09 22:31:31.044705144 +0500
+++ working     2020-10-09 22:32:18.833176354 +0500
@@ -680,7 +680,6 @@
     testfn: test::StaticTestFn(|| test::assert_test_result(issue_2022())),
 };
 fn issue_2022() {
-    #[clap(help_heading = "HEAD")]
     struct Opt2 {
         #[clap(long, name = "size")]
         pub b: Option<usize>,
@@ -704,7 +703,7 @@
         }
         fn augment_clap<'b>(app: ::clap::App<'b>) -> ::clap::App<'b> {
             {
-                let app = app.help_heading("HEAD");
+                let app = app;
                 let app = app.arg(
                     ::clap::Arg::new("size")
                         .takes_value(true)
```

---

_Comment by @pksunkara on 2020-10-09 17:37_

@CertainLach What do you mean? Does the example you gave in the first post doesn't fail anymore? Or do you mean that the line `#[clap(help_heading = "HEAD")]` is the one causing failure because the code passes if we remove the line?

---

_Comment by @CertainLach on 2020-10-09 17:40_

Code still panics in expanded form:
```rust
#[test]
fn issue_2022() {
    struct Opt2 {
        pub b: Option<usize>,
    }
    impl ::clap::Clap for Opt2 {}
    #[allow(dead_code, unreachable_code, unused_variables)]
    #[allow(
        clippy::style,
        clippy::complexity,
        clippy::pedantic,
        clippy::restriction,
        clippy::perf,
        clippy::deprecated,
        clippy::nursery,
        clippy::cargo
    )]
    #[deny(clippy::correctness)]
    impl ::clap::IntoApp for Opt2 {
        fn into_app<'b>() -> ::clap::App<'b> {
            Self::augment_clap(::clap::App::new("clap_derive"))
        }
        fn augment_clap<'b>(app: ::clap::App<'b>) -> ::clap::App<'b> {
            {
                let app = app.help_heading("HEAD");
                let app = app.arg(
                    ::clap::Arg::new("size")
                        .takes_value(true)
                        .validator(|s| {
                            ::std::str::FromStr::from_str(s)
                                .map(|_: usize| ())
                                .map_err(|e| e.to_string())
                        })
                        .long("b"),
                );
                app
            }
        }
    }
    #[allow(dead_code, unreachable_code, unused_variables)]
    #[allow(
        clippy::style,
        clippy::complexity,
        clippy::pedantic,
        clippy::restriction,
        clippy::perf,
        clippy::deprecated,
        clippy::nursery,
        clippy::cargo
    )]
    #[deny(clippy::correctness)]
    impl ::clap::FromArgMatches for Opt2 {
        fn from_arg_matches(matches: &::clap::ArgMatches) -> Self {
            Opt2 {
                b: matches
                    .value_of("size")
                    .map(|s| ::std::str::FromStr::from_str(s).unwrap()),
            }
        }
    }
    struct Opt {
        opt2: Opt2,
        a: i32,
    }
    impl ::clap::Clap for Opt {}
    #[allow(dead_code, unreachable_code, unused_variables)]
    #[allow(
        clippy::style,
        clippy::complexity,
        clippy::pedantic,
        clippy::restriction,
        clippy::perf,
        clippy::deprecated,
        clippy::nursery,
        clippy::cargo
    )]
    #[deny(clippy::correctness)]
    impl ::clap::IntoApp for Opt {
        fn into_app<'b>() -> ::clap::App<'b> {
            Self::augment_clap(::clap::App::new("clap_derive"))
        }
        fn augment_clap<'b>(app: ::clap::App<'b>) -> ::clap::App<'b> {
            {
                let app = app;
                let app = <Opt2 as ::clap::IntoApp>::augment_clap(app);
                let app = app.arg(
                    ::clap::Arg::new("a")
                        .takes_value(true)
                        .required(false)
                        .validator(|s| {
                            ::std::str::FromStr::from_str(s)
                                .map(|_: i32| ())
                                .map_err(|e| e.to_string())
                        })
                        .long("a")
                        .default_value("32"),
                );
                app
            }
        }
    }
    #[allow(dead_code, unreachable_code, unused_variables)]
    #[allow(
        clippy::style,
        clippy::complexity,
        clippy::pedantic,
        clippy::restriction,
        clippy::perf,
        clippy::deprecated,
        clippy::nursery,
        clippy::cargo
    )]
    #[deny(clippy::correctness)]
    impl ::clap::FromArgMatches for Opt {
        fn from_arg_matches(matches: &::clap::ArgMatches) -> Self {
            Opt {
                opt2: ::clap::FromArgMatches::from_arg_matches(matches),
                a: matches
                    .value_of("a")
                    .map(|s| ::std::str::FromStr::from_str(s).unwrap())
                    .unwrap(),
            }
        }
    }
    Opt::parse_from(&["test"]);
}
```
And works fine if i remove `.help_heading("HEAD")` call in them

---

_Comment by @pksunkara on 2020-10-09 17:43_

Yeah, it means we have a bug in derive because the code shouldn't be expanded like that so that it panics.

---

_Comment by @CertainLach on 2020-10-09 17:58_

Well, i was able to reduce this to 

```rust
// Panics
#[test]
fn issue_2022_heading() {
    let app = App::new("test").help_heading("HEAD").arg(
        Arg::new("a")
            .takes_value(true)
            .required(false)
            .validator(|s| {
                FromStr::from_str(s)
                    .map(|_: i32| ())
                    .map_err(|e| e.to_string())
            })
            .long("a")
            .default_value("32"),
    );
    let matches = app.get_matches_from(&["test"]);
    let _: i32 = matches
        .value_of("a")
        .map(|s| FromStr::from_str(s).unwrap())
        .unwrap();
}

// Works as intended
#[test]
fn issue_2022() {
    let app = App::new("test").arg(
        Arg::new("a")
            .takes_value(true)
            .required(false)
            .validator(|s| {
                FromStr::from_str(s)
                    .map(|_: i32| ())
                    .map_err(|e| e.to_string())
            })
            .long("a")
            .default_value("32"),
    );
    let matches = app.get_matches_from(&["test"]);
    let _: i32 = matches
        .value_of("a")
        .map(|s| FromStr::from_str(s).unwrap())
        .unwrap();
}
```

---

_Comment by @pksunkara on 2020-10-09 18:00_

Can you try to remove stuff from the first test to get a more minimal example?

---

_Referenced in [clap-rs/clap#2161](../../clap-rs/clap/pulls/2161.md) on 2020-10-09 18:25_

---

_Closed by @bors[bot] on 2020-10-11 09:27_

---

_Referenced in [clap-rs/clap#2179](../../clap-rs/clap/pulls/2179.md) on 2020-10-19 15:56_

---

_Referenced in [CertainLach/jrsonnet#18](../../CertainLach/jrsonnet/pulls/18.md) on 2020-10-19 16:23_

---

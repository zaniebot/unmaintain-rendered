```yaml
number: 3936
title: "`default_value_if` / `required_unless` broken between v3.1.17 and v3.2.11"
type: issue
state: closed
author: saschagrunert
labels:
  - C-bug
  - S-duplicate
assignees: []
created_at: 2022-07-14T11:51:03Z
updated_at: 2022-07-16T02:29:32Z
url: https://github.com/clap-rs/clap/issues/3936
synced_at: 2026-01-12T16:14:15Z
```

# `default_value_if` / `required_unless` broken between v3.1.17 and v3.2.11

---

_@saschagrunert_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

v3.2.11

### Minimal reproducible code

Using the latest `main` (5f3451efdbbd379133e6abf27c1054b0f0d33b45) of https://github.com/containers/conmon-rs:

```
> git clone https://github.com/containers/conmon-rs
> cd conmon-rs
> cargo run --bin conmonrs -- --version
…
version: 0.1.0-dev
tag: none
commit: 09b73705a78137988ac24d80a09f60e8dcdc555b
build: 2022-07-13 12:02:42 +02:00
rustc 1.62.0 (a8314ef7d 2022-06-27)
```

#### Applying the update patch

```diff
diff --git a/Cargo.lock b/Cargo.lock
index fab944a..16634b2 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -194,16 +194,16 @@ dependencies = [
 
 [[package]]
 name = "clap"
-version = "3.1.17"
+version = "3.2.11"
 source = "registry+https://github.com/rust-lang/crates.io-index"
-checksum = "47582c09be7c8b32c0ab3a6181825ababb713fde6fff20fc573a3870dd45c6a0"
+checksum = "d646c7ade5eb07c4aa20e907a922750df0c448892513714fd3e4acbc7130829f"
 dependencies = [
  "atty",
  "bitflags",
  "clap_derive",
  "clap_lex",
  "indexmap",
- "lazy_static",
+ "once_cell",
  "strsim",
  "termcolor",
  "terminal_size",
@@ -212,9 +212,9 @@ dependencies = [
 
 [[package]]
 name = "clap_derive"
-version = "3.1.7"
+version = "3.2.7"
 source = "registry+https://github.com/rust-lang/crates.io-index"
-checksum = "a3aab4734e083b809aaf5794e14e756d1c798d2c69c7f7de7a09a2f5214993c1"
+checksum = "759bf187376e1afa7b85b959e6a664a3e7a95203415dba952ad19139e798f902"
 dependencies = [
  "heck",
  "proc-macro-error",
diff --git a/conmon-rs/server/Cargo.toml b/conmon-rs/server/Cargo.toml
index 19c8bc9..5be6255 100644
--- a/conmon-rs/server/Cargo.toml
+++ b/conmon-rs/server/Cargo.toml
@@ -13,7 +13,7 @@ capnp = "0.14.7"
 capnp-rpc = "0.14.1"
 chrono = "0.4.19"
 conmon-common = { path = "../common" }
-clap = { version = "3.1.17", features = ["cargo", "derive", "env", "wrap_help"] }
+clap = { version = "3.2.11", features = ["cargo", "derive", "env", "wrap_help"] }
 futures = "0.3.21"
 getset = "0.1.2"
 serde = { version = "1.0.139", features = ["derive"] }
```


### Steps to reproduce the bug with the above code

```
> cargo run --bin conmonrs -- --version
     Updating crates.io index
     …
     Running `target/debug/conmonrs --version`
error: The following required arguments were not provided:
    --runtime <RUNTIME>
    --runtime-dir <RUNTIME_DIR>

USAGE:
    conmonrs --runtime <RUNTIME> --runtime-dir <RUNTIME_DIR> --version

For more information try --help
```

### Actual Behaviour

clap complains that `--runtime` and `--runtime-dir` are required.

### Expected Behaviour

That the version output gets printed.

### Additional Context

When changing both `default_value_if` in: 

- https://github.com/containers/conmon-rs/blob/cb91a2427172bcd5061e89b3b8503ca79601637e/conmon-rs/server/src/config.rs#L55
- https://github.com/containers/conmon-rs/blob/cb91a2427172bcd5061e89b3b8503ca79601637e/conmon-rs/server/src/config.rs#L66

to `required_unless` via:

```diff
diff --git a/conmon-rs/server/src/config.rs b/conmon-rs/server/src/config.rs
index 333540b..7f242e2 100644
--- a/conmon-rs/server/src/config.rs
+++ b/conmon-rs/server/src/config.rs
@@ -52,7 +52,7 @@ pub struct Config {
 
     #[get = "pub"]
     #[clap(
-        default_value_if("version", None, Some("")),
+        required_unless("version"),
         env(concat!(prefix!(), "RUNTIME")),
         long("runtime"),
         short('r'),
@@ -63,7 +63,7 @@ pub struct Config {
 
     #[get = "pub"]
     #[clap(
-        default_value_if("version", None, Some("")),
+        required_unless("version"),
         env(concat!(prefix!(), "RUNTIME_DIR")),
         long("runtime-dir"),
         value_name("RUNTIME_DIR")
```

has the same effect.

### Debug Output

```
   Compiling conmon v0.1.0-dev (/home/sascha/git/conmon-rs/conmon-rs/server)
    Finished dev [unoptimized + debuginfo] target(s) in 7.42s
     Running `target/debug/conmonrs --version`
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="conmon"
[      clap::builder::command]  Command::_propagate:conmon
[      clap::builder::command]  Command::_check_help_and_version: conmon
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version
[      clap::builder::command]  Command::_propagate_global_args:conmon
[      clap::builder::command]  Command::_derive_display_order:conmon
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Arg::_debug_asserts:log-level
[clap::builder::debug_asserts]  Arg::_debug_asserts:log-driver
[clap::builder::debug_asserts]  Arg::_debug_asserts:runtime
[clap::builder::debug_asserts]  Arg::_debug_asserts:runtime-dir
[clap::builder::debug_asserts]  Arg::_debug_asserts:runtime-root
[clap::builder::debug_asserts]  Arg::_debug_asserts:skip-fork
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--version")' ([45, 45, 118, 101, 114, 115, 105, 111, 110])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("--version")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_long_arg
[        clap::parser::parser]  Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser]  Parser::parse_long_arg: Found valid arg or flag '--version'
[        clap::parser::parser]  Parser::parse_long_arg("version"): Presence validated
[        clap::parser::parser]  Parser::react action=IncOccurrence, identifier=Some(Long), source=CommandLine
[        clap::parser::parser]  Parser::react: cur_idx:=1
[        clap::parser::parser]  Parser::remove_overrides: id=version
[   clap::parser::arg_matcher]  ArgMatcher::start_occurrence_of_arg: id=version
[      clap::builder::command]  Command::groups_for_arg: id=version
[        clap::parser::parser]  Parser::get_matches_with: After parse_long_arg ValuesDone
[        clap::parser::parser]  Parser::add_env
[        clap::parser::parser]  Parser::add_env: Checking arg `--help`
[        clap::parser::parser]  Parser::add_env: Skipping existing arg `--version`
[        clap::parser::parser]  Parser::add_env: Checking arg `--log-level <LEVEL>`
[        clap::parser::parser]  Parser::add_env: Checking arg `--log-driver <DRIVER>`
[        clap::parser::parser]  Parser::add_env: Checking arg `--runtime <RUNTIME>`
[        clap::parser::parser]  Parser::add_env: Checking arg `--runtime-dir <RUNTIME_DIR>`
[        clap::parser::parser]  Parser::add_env: Checking arg `--runtime-root <RUNTIME_ROOT>`
[        clap::parser::parser]  Parser::add_env: Checking arg `--skip-fork <SKIP_FORK>`
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:version:
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:log-level:
[        clap::parser::parser]  Parser::add_default_value:iter:log-level: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:log-level: has default vals
[        clap::parser::parser]  Parser::add_default_value:iter:log-level: wasn't used
[        clap::parser::parser]  Parser::split_arg_values; arg=log-level, val=RawOsStr("info")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::react action=StoreValue, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id=log-level, source=DefaultValue
[      clap::builder::command]  Command::groups_for_arg: id=log-level
[        clap::parser::parser]  Parser::push_arg_values: ["info"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command]  Command::groups_for_arg: id=log-level
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=log-level, resolved=1, pending=0
[        clap::parser::parser]  Parser::add_defaults:iter:log-driver:
[        clap::parser::parser]  Parser::add_default_value:iter:log-driver: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:log-driver: has default vals
[        clap::parser::parser]  Parser::add_default_value:iter:log-driver: wasn't used
[        clap::parser::parser]  Parser::split_arg_values; arg=log-driver, val=RawOsStr("systemd")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::react action=StoreValue, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id=log-driver, source=DefaultValue
[      clap::builder::command]  Command::groups_for_arg: id=log-driver
[        clap::parser::parser]  Parser::push_arg_values: ["systemd"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=3
[      clap::builder::command]  Command::groups_for_arg: id=log-driver
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=log-driver, resolved=1, pending=0
[        clap::parser::parser]  Parser::add_defaults:iter:runtime:
[        clap::parser::parser]  Parser::add_default_value:iter:runtime: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: has conditional defaults
[        clap::parser::parser]  Parser::split_arg_values; arg=runtime, val=RawOsStr("")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::react action=StoreValue, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id=runtime, source=DefaultValue
[      clap::builder::command]  Command::groups_for_arg: id=runtime
[        clap::parser::parser]  Parser::push_arg_values: [""]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[      clap::builder::command]  Command::groups_for_arg: id=runtime
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=runtime, resolved=1, pending=0
[        clap::parser::parser]  Parser::add_defaults:iter:runtime-dir:
[        clap::parser::parser]  Parser::add_default_value:iter:runtime-dir: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: has conditional defaults
[        clap::parser::parser]  Parser::split_arg_values; arg=runtime-dir, val=RawOsStr("")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::react action=StoreValue, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id=runtime-dir, source=DefaultValue
[      clap::builder::command]  Command::groups_for_arg: id=runtime-dir
[        clap::parser::parser]  Parser::push_arg_values: [""]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=5
[      clap::builder::command]  Command::groups_for_arg: id=runtime-dir
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=runtime-dir, resolved=1, pending=0
[        clap::parser::parser]  Parser::add_defaults:iter:runtime-root:
[        clap::parser::parser]  Parser::add_default_value:iter:runtime-root: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:runtime-root: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:skip-fork:
[        clap::parser::parser]  Parser::add_default_value:iter:skip-fork: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:skip-fork: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_conflicts::iter: id=version
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=version
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([Child { id: runtime, children: [] }, Child { id: runtime-dir, children: [] }])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::gather_requires:iter:version
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator]  Validator::validate_required:iter:aog=runtime
[     clap::parser::validator]  Validator::validate_required:iter: This is an arg
[     clap::parser::validator]  Validator::is_missing_required_ok: runtime
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=runtime
[      clap::builder::command]  Command::groups_for_arg: id=runtime
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id=runtime, conflicts=[]
[      clap::builder::command]  Command::groups_for_arg: id=version
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id=version, conflicts=[]
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::missing_required_error; incl=[]
[     clap::parser::validator]  Validator::missing_required_error: reqs=ChildGraph([Child { id: runtime, children: [] }, Child { id: runtime-dir, children: [] }])
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={runtime, runtime-dir}
[         clap::output::usage]  Usage::get_required_usage_from:iter:runtime
[         clap::output::usage]  Usage::get_required_usage_from:iter:runtime-dir
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={"--runtime <RUNTIME>", "--runtime-dir <RUNTIME_DIR>"}
[     clap::parser::validator]  Validator::missing_required_error: req_args=[
    "--runtime <RUNTIME>",
    "--runtime-dir <RUNTIME_DIR>",
]
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_smart_usage
[         clap::output::usage]  Usage::get_required_usage_from: incls=[version], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={runtime, runtime-dir}
[         clap::output::usage]  Usage::get_required_usage_from:iter:runtime
[         clap::output::usage]  Usage::get_required_usage_from:iter:runtime-dir
[         clap::output::usage]  Usage::get_required_usage_from:iter:version
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={"--runtime <RUNTIME>", "--runtime-dir <RUNTIME_DIR>", "--version"}
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: The following required arguments were not provided:
    --runtime <RUNTIME>
    --runtime-dir <RUNTIME_DIR>

USAGE:
    conmonrs --runtime <RUNTIME> --runtime-dir <RUNTIME_DIR> --version

For more information try --help

```

---

_Label `C-bug` added by @saschagrunert on 2022-07-14 11:51_

---

_Comment by @epage on 2022-07-16 02:29_

This was reported earlier as #3838

---

_Closed by @epage on 2022-07-16 02:29_

---

_Label `S-duplicate` added by @epage on 2022-07-16 02:29_

---

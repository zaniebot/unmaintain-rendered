```yaml
number: 2540
title: "Panic, `unwrap()` on a `None` value"
type: issue
state: closed
author: mazznoer
labels:
  - C-bug
assignees: []
created_at: 2021-06-16T13:49:25Z
updated_at: 2021-06-16T13:50:34Z
url: https://github.com/clap-rs/clap/issues/2540
synced_at: 2026-01-12T16:14:13Z
```

# Panic, `unwrap()` on a `None` value

---

_@mazznoer_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.50.0 (cb75ad5db 2021-02-10)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

`Cargo.toml`:

```toml
[package]
name = "test"
version = "0.1.0"
edition = "2018"

[dependencies]
clap = { version = "3.0.0-beta.2" }
```

`main.rs`:

```rust
use clap::Clap;

#[derive(Debug, Clap)]
struct Opt {
    #[clap(short, long, default_value = "xxx", value_name = "NAME", help_heading = Some("HEADING-1"))]
    name: String,
}

fn main() {
    let _opt = Opt::parse();
}
```


### Steps to reproduce the bug with the above code

```
$ cargo build
$ target/debug/test
```

### Actual Behaviour

Panic.

### Expected Behaviour

Not panicking.

### Additional Context

_No response_

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:test
[            clap::build::app] 	App::_derive_display_order:test
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:name
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
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
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
```

---

_Label `T: bug` added by @mazznoer on 2021-06-16 13:49_

---

_Comment by @pksunkara on 2021-06-16 13:50_

Not enough information in the actual behaviour. Also, try master.

---

_Closed by @pksunkara on 2021-06-16 13:50_

---

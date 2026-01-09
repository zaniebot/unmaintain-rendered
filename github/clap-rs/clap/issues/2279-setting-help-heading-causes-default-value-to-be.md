---
number: 2279
title: Setting help_heading causes default_value to be ignored for subsequent named arguments
type: issue
state: closed
author: wizhi
labels:
  - C-bug
assignees: []
created_at: 2020-12-29T00:36:59Z
updated_at: 2021-01-20T18:04:38Z
url: https://github.com/clap-rs/clap/issues/2279
synced_at: 2026-01-07T13:12:19-06:00
---

# Setting help_heading causes default_value to be ignored for subsequent named arguments

---

_Issue opened by @wizhi on 2020-12-29 00:36_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
use clap::{App, Arg};

fn main() {
    let before_help_heading = App::new("app")
        .arg(Arg::new("foo").short('f').default_value("bar"))
        .help_heading("This causes default_value to be ignored")
        .get_matches();

    assert_eq!(before_help_heading.value_of("foo"), Some("bar"));

    let after_help_heading = App::new("app")
        .help_heading("This causes default_value to be ignored")
        .arg(Arg::new("foo").short('f').default_value("bar"))
        .get_matches();

    assert_ne!(after_help_heading.value_of("foo"), Some("bar"));
}
```

### Steps to reproduce the issue

1. Run `cargo run --`

### Version

* **Rust**: `rustc 1.47.0 (18bf6b4f0 2020-10-07)`
* **Clap**: `3.0.0-beta.2`

### Actual Behavior Summary

When I specify a `help_heading()`, any subsequent **named** (`short()` _or_ `long()`) arguments, which specify a `default_value()`, will not have their default value available through `value_of()`.

### Expected Behavior Summary

I believe that `help_heading` isn't meant to have any effect on argument functionality.
From what I can tell, it should purely change the `--help` output layout.

### Additional context

Positional (no `short()` _or_ `long()`) arguments seem unaffected.

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:app
[            clap::build::app] 	App::_derive_display_order:app
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:foo
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:foo:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:foo: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:foo: wasn't used
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=foo, val="bar"
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."bar"
[            clap::build::app] 	App::groups_for_arg: id=foo
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=foo
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:foo
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::gather_conflicts:iter: id=foo
[      clap::parse::validator] 	Validator::gather_conflicts:iter: This is default value, skipping.
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:foo
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[      clap::parse::validator] 	Validator::validate_matched_args:iter:foo: vals=[
    "bar",
]
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="foo"
[      clap::parse::validator] 	Validator::validate_arg_requires:"foo"
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "foo"=0
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:app
[            clap::build::app] 	App::_derive_display_order:app
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:foo
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
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:app
[            clap::build::app] 	App::_derive_display_order:app
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:foo
[            clap::build::arg] 	Arg::_debug_asserts:baz
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

</code>
</pre>
</details>


---

_Label `T: bug` added by @wizhi on 2020-12-29 00:36_

---

_Added to milestone `3.0` by @pksunkara on 2020-12-30 03:59_

---

_Label `C: args` added by @pksunkara on 2020-12-30 04:00_

---

_Comment by @omar25h on 2020-12-31 19:42_

This seems to be already fixed on the `master` branch.
I tried running the example provided with the crate from [crates.io](https://crates.io/crates/clap/3.0.0-beta.2). The example ran without panics.
However, running the example on the current `master` branch (with cargo path override) seems to report the following panic output:
```
thread 'main' panicked at 'assertion failed: `(left != right)`
  left: `Some("bar")`,
 right: `Some("bar")`', src/main.rs:16:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @pksunkara on 2021-01-01 06:19_

Left and right both looks the same to me.

---

_Comment by @omar25h on 2021-01-02 10:40_

yes they are, but the statement asserts that they shouldn't be equal, so that the example runs without any panics on `3.0.0-beta.2` (hence the bug)

---

_Comment by @ldm0 on 2021-01-20 17:06_

This panic seems to be the right behaviour. The original problem is "Setting help_heading causes default_value to be ignored", so the first assert passes and the second panics is the right behaviour.

---

_Referenced in [clap-rs/clap#2303](../../clap-rs/clap/pulls/2303.md) on 2021-01-20 17:13_

---

_Closed by @pksunkara on 2021-01-20 18:04_

---

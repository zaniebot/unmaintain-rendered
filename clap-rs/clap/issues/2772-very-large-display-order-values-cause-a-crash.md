```yaml
number: 2772
title: Very large display_order values cause a crash while displaying help
type: issue
state: closed
author: tangmi
labels:
  - C-bug
assignees: []
created_at: 2021-09-17T02:10:35Z
updated_at: 2021-09-17T18:39:38Z
url: https://github.com/clap-rs/clap/issues/2772
synced_at: 2026-01-12T16:14:13Z
```

# Very large display_order values cause a crash while displaying help

---

_@tangmi_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

3.0.0-beta.4

### Minimal reproducible code

```rust
fn main() {
    clap::App::new("test")
        .subcommand(clap::App::new("sub").display_order(usize::MAX))
        .print_help()
        .unwrap();
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

Results in the following crash in `VecMap`:

```
thread 'main' panicked at 'attempt to add with overflow', ~/.cargo/registry/src/github.com-1ecc6299db9ec823/vec_map-0.8.2/src/lib.rs:540:31
```

Using `usize::MAX - 1` results in a `'capacity overflow'` in `VecMap`:
```
thread 'main' panicked at 'capacity overflow', library/alloc/src/raw_vec.rs:559:5
```

### Expected Behaviour

The help output is printed.

### Additional Context

I believe this was previously working for me, but I'm not sure when. I have upgraded Rust a couple times (it was working maybe in 1.52?), but I'm unsure if that's the only thing has changed.

### Debug Output

```
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:test
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Building help subcommand
[            clap::build::app] 	App::_propagate_global_args:test
[            clap::build::app] 	App::_derive_display_order:test
[            clap::build::app] 	App::_derive_display_order:sub
[            clap::build::app] 	App::_derive_display_order:help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::color_help
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_before_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_flags_tag
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=version
[         clap::output::usage] 	Usage::needs_flags_tag: [FLAGS] not required
[         clap::output::usage] 	Usage::create_help_usage: usage=test [SUBCOMMAND]
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::write_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args: Current Longest...2
[          clap::output::help] 	Help::write_args: New Longest...6
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::write_args: Current Longest...6
[          clap::output::help] 	Help::write_args: New Longest...9
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::spec_vals: a=--version
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=help
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::spec_vals: a=--version
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=version
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommands
thread 'main' panicked at 'attempt to add with overflow', ~/.cargo/registry/src/github.com-1ecc6299db9ec823/vec_map-0.8.2/src/lib.rs:540:31
```

---

_Label `T: bug` added by @tangmi on 2021-09-17 02:10_

---

_Referenced in [clap-rs/clap#2773](../../clap-rs/clap/pulls/2773.md) on 2021-09-17 02:12_

---

_Closed by @pksunkara on 2021-09-17 18:39_

---

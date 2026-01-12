```yaml
number: 4100
title: Help subcommand argument should not have ellipsis
type: issue
state: closed
author: patrick-gu
labels:
  - C-bug
assignees: []
created_at: 2022-08-20T18:58:24Z
updated_at: 2023-04-03T14:44:27Z
url: https://github.com/clap-rs/clap/issues/4100
synced_at: 2026-01-12T16:14:15Z
```

# Help subcommand argument should not have ellipsis

---

_@patrick-gu_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

3.2.17

### Minimal reproducible code

```rust
fn main() {
    let app = clap::Command::new("app").subcommand(clap::Command::new("sub"));

    app.get_matches_from(["program", "help", "help"]);
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The output is:
```
program-help 
Print this message or the help of the given subcommand(s)

USAGE:
    program help [SUBCOMMAND]...

ARGS:
    <SUBCOMMAND>...    The subcommand whose help message to display
```
I believe it is incorrect to have the ellipsis after `[SUBCOMMAND]` and `<SUBCOMMAND>` because the value for the `help` subcommand can be specified at most once. 

### Expected Behaviour

The output should be:
```
program-help 
Print this message or the help of the given subcommand(s)

USAGE:
    program help [SUBCOMMAND]

ARGS:
    <SUBCOMMAND>    The subcommand whose help message to display
```

### Additional Context

I have a prepared a fix [here](https://github.com/patrick-gu/clap).

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="app"
[      clap::builder::command]  Command::_propagate:app
[      clap::builder::command]  Command::_check_help_and_version: app
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version     
[      clap::builder::command]  Command::_check_help_and_version: Building help subcommand       
[      clap::builder::command]  Command::_propagate_global_args:app
[      clap::builder::command]  Command::_propagate removing sub's help
[      clap::builder::command]  Command::_propagate pushing help to sub
[      clap::builder::command]  Command::_propagate removing help's help
[      clap::builder::command]  Command::_propagate pushing help to help
[      clap::builder::command]  Command::_derive_display_order:app
[      clap::builder::command]  Command::_derive_display_order:sub
[      clap::builder::command]  Command::_derive_display_order:help
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("help")' ([104, 101, 108, 112])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("help")
[        clap::parser::parser]  Parser::get_matches_with: sc=Some("help")
[        clap::parser::parser]  Parser::parse_help_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={}
[      clap::builder::command]  Command::_build_subcommand Setting bin_name of help to "program help"
[      clap::builder::command]  Command::_build_subcommand Setting display_name of help to "app-help"
[      clap::builder::command]  Command::_build: name="help"
[      clap::builder::command]  Command::_propagate:help
[      clap::builder::command]  Command::_check_help_and_version: help
[      clap::builder::command]  Command::_check_help_and_version: Removing generated help        
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version     
[      clap::builder::command]  Command::_propagate_global_args:help
[      clap::builder::command]  Command::_derive_display_order:help
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:subcommand
[clap::builder::debug_asserts]  Command::_verify_positionals
[      clap::builder::command]  Command::use_long_help
[      clap::builder::command]  Parser::write_help_err: use_long=false, stream=Stdout
[      clap::builder::command]  Command::use_long_help
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[          clap::output::help]  Help::new cmd=help, use_long=false
[          clap::output::help]  Help::write_help
[          clap::output::help]  should_show_arg: use_long=false, arg=subcommand
[          clap::output::help]  Help::write_templated_help
[          clap::output::help]  Help::write_before_help
[          clap::output::help]  Help::write_bin_name
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={}
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::get_args_tag; incl_reqs = true
[         clap::output::usage]  Usage::get_args_tag:iter:subcommand
[      clap::builder::command]  Command::groups_for_arg: id=subcommand
[         clap::output::usage]  Usage::get_args_tag:iter: 1 Args not required or hidden
[      clap::builder::command]  Command::groups_for_arg: id=subcommand
[         clap::output::usage]  Usage::get_args_tag:iter: Exactly one, returning 'subcommand'    
[          clap::builder::arg]  Arg::name_no_brackets:subcommand
[          clap::builder::arg]  Arg::name_no_brackets: val_names=[
    "SUBCOMMAND",
]
[         clap::output::usage]  Usage::create_help_usage: usage=program help [SUBCOMMAND]...     
[          clap::output::help]  Help::write_all_args
[          clap::output::help]  should_show_arg: use_long=false, arg=subcommand
[          clap::output::help]  Help::write_args_unsorted
[          clap::output::help]  should_show_arg: use_long=false, arg=subcommand
[          clap::output::help]  Help::write_args_unsorted: arg="subcommand" longest=15
[          clap::output::help]  should_show_arg: use_long=false, arg=subcommand
[          clap::output::help]  Help::spec_vals: a=<SUBCOMMAND>...
[          clap::output::help]  Help::spec_vals: a=<SUBCOMMAND>...
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=subcommand
[          clap::output::help]  Help::align_to_about: arg=subcommand, next_line_help=false, longest=15
[          clap::output::help]  Help::align_to_about: positional=true arg_len=15, spaces=4       
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_after_help
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
program-help
Print this message or the help of the given subcommand(s)

USAGE:
    program help [SUBCOMMAND]...

ARGS:
    <SUBCOMMAND>...    The subcommand whose help message to display
```

---

_Label `C-bug` added by @patrick-gu on 2022-08-20 18:58_

---

_Referenced in [clap-rs/clap#4101](../../clap-rs/clap/pulls/4101.md) on 2022-08-20 19:03_

---

_Comment by @epage on 2022-08-20 19:06_

> I believe it is incorrect to have the ellipsis after [SUBCOMMAND] and <SUBCOMMAND> because the value for the help subcommand can be specified at most once.

You can specify as many subcommands as you want.

See https://github.com/clap-rs/clap/blob/master/tests/builder/help.rs#L76 for an example

Now, the question may be whether the application has any more subcommands to do nesting.  Unfortunately, the cost of calculating the depth of nested subcommands isn't worth it.  The cost of making this configurable is also unlikely to be worth it.

So the only question remaining is which direction should it be biased for.  I think the fact that this issue was opened helped to fulfill its purpose.  Without `...` it wouldn;'t have even come up on whether multiple are allowed or not.

---

_Comment by @patrick-gu on 2022-08-20 19:20_

I didn't realize it was possible to get help for nested subcommands. To me, the description `Print this message or the help of the given subcommand(s)` made it appear that you can, for example, get help for both `program help` and `program sub` at the same time, when in reality you can only specify one, but it may be nested, like `program sub1 sub2`. However, that might just be my interpretation.

I think the ability to get help for nested subcommands is useful. It just wasn't obvious to me that that was the purpose of the ellipsis.

---

_Comment by @epage on 2022-08-20 23:42_

Some things that would help is completion support for those using it, see https://github.com/clap-rs/clap/issues/2315

---

_Closed by @epage on 2022-08-20 23:42_

---

_Referenced in [clap-rs/clap#4342](../../clap-rs/clap/issues/4342.md) on 2022-10-04 15:21_

---

_Comment by @kevlarr on 2023-04-01 02:44_

@epage I like having the `help` sub-command baked in by default but I wish I could either edit its description *or* hide it from the "subcommands" list completely and add my own description & usage example of it elsewhere, without changing the functionality or having to define it myself.

Take `git` as an example - `help` is only mentioned as a distinct command in what amounts to the 'after help' (to use `clap` terminology) block. There's a certain logic to excluding it from sub-command lists, too, since it's not a 'real' sub-command in the sense that it's not used to perform any actual operation.

Being able to edit the description (and possibly location of description) for the `help` sub-command could help to avoid the confusion on how to use it, especially because it could open up the opportunity to show some good examples of using `help` with nested sub-commands specific to an individual project.

---

_Comment by @epage on 2023-04-03 14:44_

In terms of this issue, my priority would be on improving things centrally so everyone benefits.

In terms of wider feature support, we have talked about Command actions, similar to `ArgAction`.   Like with `--help`, this would allow disabling the built-in command while still getting a command to have the built-in behavior.  This talk has been vague enough that I don't even think we have an issue for it yet.

---

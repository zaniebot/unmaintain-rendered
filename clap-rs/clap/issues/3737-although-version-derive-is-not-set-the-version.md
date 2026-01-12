```yaml
number: 3737
title: "Although `version` derive is not set, the `--version` logic is exist"
type: issue
state: closed
author: greenhat616
labels:
  - C-bug
assignees: []
created_at: 2022-05-19T11:15:35Z
updated_at: 2022-05-19T19:26:36Z
url: https://github.com/clap-rs/clap/issues/3737
synced_at: 2026-01-12T16:14:15Z
```

# Although `version` derive is not set, the `--version` logic is exist

---

_@greenhat616_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0-nightly (cd282d7f7 2022-05-18)

### Clap Version

3.1.18

### Minimal reproducible code

```rust
use clap::Parser;
use std::error::Error;
use colored::Colorize;

#[derive(Parser, Debug)]
#[clap(about, long_about = None)]
pub struct Args {
    #[clap(short = 'D', long, help = "Run in development mode")]
    pub dev: bool, // development mode
    #[clap(short, long, help = "Specify config file path")]
    pub config_path: Option<String>, // manual set config path
    #[clap(short, long, help = "Print version information")]
    pub version: bool, // show version info
}

pub enum Command {
    Test,
}

fn print_version() -> () {
    println!("{} v{}", env!("CARGO_PKG_NAME").bright_black(), env!("CARGO_PKG_VERSION").red());
}

pub fn handle_args() -> Result<Args, Box<dyn Error>> {
    let args = Args::parse(); // this line will block the `--version` process and exit program with 0
    if args.version { // this line will not run
        print_version();
        std::process::exit(0);
    }
    Ok(args)
}
```

### Steps to reproduce the bug with the above code

`cargo run -- --version`

### Actual Behaviour

While run `cargo run -- --version` the command get processed by clap lib itself. It will ruturn `PKG_NAME ` with no version specified and exit.

LIKE :
```
hitokoto_reviewer 
```


### Expected Behaviour

It should works like:
![image](https://user-images.githubusercontent.com/41122242/169280521-812cccc3-66bc-43c9-bafa-ef9d98a2d5c4.png)

### Additional Context

Because `derive mode` is not support dynamic string/function, we should to use a custom `version` flag to run custom code.

### Debug Output

```
warning: `hitokoto_reviewer` (bin "hitokoto_reviewer") generated 3 warnings
    Finished dev [unoptimized + debuginfo] target(s) in 0.64s
     Running `target\debug\hitokoto_reviewer.exe --version`
[        clap::build::command]  Command::_do_parse
[        clap::build::command]  Command::_build: name="hitokoto_reviewer"
[        clap::build::command]  Command::_propagate:hitokoto_reviewer
[        clap::build::command]  Command::_check_help_and_version: hitokoto_reviewer
[        clap::build::command]  Command::_check_help_and_version: Removing generated version
[        clap::build::command]  Command::_propagate_global_args:hitokoto_reviewer
[        clap::build::command]  Command::_derive_display_order:hitokoto_reviewer
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Arg::_debug_asserts:dev
[  clap::build::debug_asserts]  Arg::_debug_asserts:config-path
[  clap::build::debug_asserts]  Arg::_debug_asserts:version
[  clap::build::debug_asserts]  Command::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--version")' ([45, 45, 118, 101, 114, 115, 105, 111, 110])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=Ok("--version")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--version'
[         clap::parse::parser]  Parser::check_for_help_and_version_str
[         clap::parse::parser]  Parser::check_for_help_and_version_str: Checking if --RawOsStr("version") is help or version...
[         clap::parse::parser]  Version
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg VersionFlag
[         clap::parse::parser]  Parser::version_err
[        clap::build::command]  Command::_render_version
[        clap::build::command]  Command::color: Color setting...
[        clap::build::command]  Auto
[        clap::build::command]  Command::color: Color setting...
[        clap::build::command]  Auto
hitokoto_reviewer
```

---

_Label `C-bug` added by @greenhat616 on 2022-05-19 11:15_

---

_Renamed from "While `version` derive is not set, the `--version` logic is exist yet" to "Although `version` derive is not set, the `--version` logic is exist" by @greenhat616 on 2022-05-19 11:16_

---

_Comment by @epage on 2022-05-19 12:33_

>While run cargo run -- --version the command get processed by clap lib itself. It will ruturn PKG_NAME with no version specified and exit.
>
> LIKE :
>
> `hitokoto_reviewer`

I can see in `clap v4` to drop the `--version` flag if no version is specified.

> It should works like:
> `hitokoto_reviewer  v0.1.0`

When transitioning from structopt, we intentionally stopped implicitly reading the manifest's version and adding it, instead requiring users to specify `version` (no parameter) to opt-in to reading the manifest.  In structopt, reading it implicitly led to having to need an opt-out via `no_version` and we'd rather not have that.

> Because derive mode is not support dynamic string/function,

If it doesn't support a function call, then that is a bug that should be reported

As for runtime strings, we plan to fix that with https://github.com/clap-rs/clap/issues/2150.  A short-term workaround is to leak the memory to give it a `'static` lifetime (`clap_derive` actually does this under the hood in a couple of places)

> we should to use a custom version flag to run custom code.

We have [NoAutoVersion](https://docs.rs/clap/latest/clap/enum.AppSettings.html#variant.NoAutoVersion) for disabling the built-in semantics when a version flag is specified by the user so you can provide your own.  I can't remember if it will still auto-add a version flag or not.  With https://github.com/clap-rs/clap/issues/3405, we plan to add `Action`s to define the behavior of a flag.  At some point, we expect to support custom actions where the user can do what they want, including whatever clap does natively.



---

_Comment by @greenhat616 on 2022-05-19 13:51_

> 

Thanks for your patient reply.
I have tried the measures you referred. **They are all work!** How kind you are. Sincerely thank you for your help.

> We have [NoAutoVersion](https://docs.rs/clap/latest/clap/enum.AppSettings.html#variant.NoAutoVersion) for disabling the built-in semantics when a version flag is specified by the user so you can provide your own. I can't remember if it will still auto-add a version flag or not. With https://github.com/clap-rs/clap/issues/3405, we plan to add Actions to define the behavior of a flag. At some point, we expect to support custom actions where the user can do what they want, including whatever clap does natively. 

With `global_setting(AppSettings::NoAutoVersion)` is set, we can process the logic by myself. It will not auto add a version flag #:smiley:

> If it doesn't support a function call, then that is a bug that should be reported
> 
> As for runtime strings, we plan to fix that with https://github.com/clap-rs/clap/issues/2150. A short-term workaround is to leak the memory to give it a 'static lifetime (clap_derive actually does this under the hood in a couple of places)

It's my fault. The custom function is work. What make me wrong is that the custom function should return `&'static str`. I think it  would be `&str`, like `serde`. So I described it as not support.

```rust
#[derive(Parser, Debug)]
#[clap(author, version = print_version(), about, long_about = None)]
pub struct Args {}

fn print_version() -> &'static str {
    Box::leak(format!("{} v{}", env!("CARGO_PKG_NAME").bright_black(), env!("CARGO_PKG_VERSION").red()).into())
}
```

I will close the issue. Thank you again.

 

---

_Closed by @greenhat616 on 2022-05-19 13:51_

---

_Comment by @epage on 2022-05-19 19:26_

> I can see in clap v4 to drop the --version flag if no version is specified.

We already do this
```rust
        // Determine if we should remove the generated --version flag
        //
        // Note that if only mut_arg() was used, the first expression will evaluate to `true`
        // however inside the condition block, we only check for Generated args, not
        // GeneratedMutated args, so the `mut_arg("version", ..) will be skipped and fall through
        // to the following condition below (Adding the short `-V`)
        if self.settings.is_set(AppSettings::DisableVersionFlag)
            || (self.version.is_none() && self.long_version.is_none())
```

What I had missed is the reproduction case had
```rust
    #[clap(short, long, help = "Print version information")]
    pub version: bool, // show version info
```

---

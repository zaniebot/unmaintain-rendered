---
number: 5530
title: Quoted argument gets treated as multiple arguments
type: issue
state: closed
author: PacifiK2460
labels:
  - C-bug
assignees: []
created_at: 2024-06-12T15:48:32Z
updated_at: 2024-06-12T17:58:40Z
url: https://github.com/clap-rs/clap/issues/5530
synced_at: 2026-01-07T13:12:20-06:00
---

# Quoted argument gets treated as multiple arguments

---

_Issue opened by @PacifiK2460 on 2024-06-12 15:48_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29) (Windows 11, Powershell)

### Clap Version

4.5.6

### Minimal reproducible code

```rust

struct Args {
    /// The list directory to include in the archive.
    /// If not specified, the input will be the current directory
    #[arg(short, long, required = false, default_value = ".")]
    input_folder: PathBuf,

    /// The output file to write to.
    /// If not specified, the output will be the current directory
    #[arg(short, long, required = false, default_value = ".")]
    output_folder: PathBuf,

    /// Launcher for the archive.
    /// If not specified, the archive will be extracted but not executed.
    /// The launcher will be executed as a command.
    #[arg(short, long, required = false, default_value = "")]
    launch: String,
    /// The compression level to use.
    /// If not specified, the archive will have 0 compression. 9 is the highest compression level.
    #[arg(short, long, required = false, default_value = "0")]
    compress: usize,

    // Name of the executable.
    // If not specified, the name will be the same as the input folder
    #[arg(short, long, required = false)]
    name: Option<String>,

    // Self cleanup after execution?
    #[arg(short, long, required = false, default_value = "false")]
    after_cleanup: bool,
}

fn main(){
    let args = Args::parse();
    ...
}
```


### Steps to reproduce the bug with the above code

`cargo run -- -l 'powershell.exe -File .\install.ps1'`

### Actual Behaviour

```
error: unexpected argument '-F' found

Usage: propane-packer.exe [OPTIONS]

For more information, try '--help'.
```

### Expected Behaviour

Should get the `launch` argument as a single string with the value of 'powershell.exe -File .\install.ps1'

### Additional Context

Just that problem, I knew that my argument would be better to be quoted since it'll part of user input (therefore I won't know the lenght of the argument)

### Debug Output
for: `-i '.\dir\' -c 9 -l 'powershell.exe -File .\install.ps1'`
```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="propane-packer"
[clap_builder::builder::command]Command::_propagate:propane-packer
[clap_builder::builder::command]Command::_check_help_and_version:propane-packer expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:propane-packer
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:input_folder
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:output_folder
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:launch
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:compress
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:name
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:after_cleanup
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-i"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-i")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "i", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['i']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:i
[clap_builder::parser::parser]Parser::parse_short_arg:iter:i: Found valid opt or flag
[clap_builder::parser::parser]Parser::parse_short_arg:iter:i: val="", short_arg=ShortFlags { inner: "i", utf8_prefix: CharIndices { front_offset: 1, iter: Chars([]) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_opt_value; arg=input_folder, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(0)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg Opt("input_folder")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '".\\dir\" -c 9 -l powershell.exe"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=input_folder, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=1
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-File"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-File")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "File", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['F', 'i', 'l', 'e']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:F
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg NoMatchingArg { arg: "-F" }
[clap_builder::parser::parser]Parser::resolve_pending: id="input_folder"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=1
[clap_builder::parser::parser]Parser::remove_overrides: id="input_folder"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="input_folder", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="input_folder"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Args", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: [".\\dir\" -c 9 -l powershell.exe"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=input_folder, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=input_folder
[clap_builder::builder::command]Command::groups_for_arg: id="input_folder"
[ clap_builder::output::usage]Usage::needs_options_tag:iter:iter: grp_s="Args"
[ clap_builder::output::usage]Usage::needs_options_tag:iter: [OPTIONS] required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: propane-packer.exe [OPTIONS]
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @PacifiK2460 on 2024-06-12 15:48_

---

_Comment by @epage on 2024-06-12 15:53_

What OS and shell are you using?

Could you add the Debug Output?

Quoting is machine dependent.  On Windows, its usually controlled by libc/stdlib.  On other platforms, its usually controlled by the shell.  Looking at the Debug Output can confirm what clap is getting.

---

_Comment by @PacifiK2460 on 2024-06-12 17:41_

@epage, it looks like the first argument gets the correct value, but the next string argument gets the remaining arguments as a string:

If I run it with the arguments `-i '.\directory\' -c 9 -l 'powershell.exe -File .\install.ps1'`, I get `error: unexpected argument '-F' found.`

If I run it with  `-l 'powershell.exe -File .\install.ps1' -i '.\dir\' -c 9`, I get instead `ERR: ".\\dir\" -c 9" is not a directory.` (The error is from later code where I check if the input directory is existent.)

---

_Comment by @epage on 2024-06-12 17:58_

Looking at the debug output, it appears this is all a matter of how quoting works in the Rust standard library when it parses the command line.

You can confirm this by trying your inputs on
```
fn main() {
    dbg!(std::env::args());
}Could you try those with
```

As such, there isn't anything clap can or should do and I'm going to go ahead and close this.  If there is a reason to keep this re-evaluate this, let us know!

---

_Closed by @epage on 2024-06-12 17:58_

---

```yaml
number: 4282
title: Combining short flags (with subcommand short flag) produces errors
type: issue
state: open
author: ttytm
labels:
  - C-bug
  - A-parsing
  - E-medium
assignees: []
created_at: 2022-09-29T00:36:38Z
updated_at: 2022-09-29T00:55:42Z
url: https://github.com/clap-rs/clap/issues/4282
synced_at: 2026-01-12T16:14:15Z
```

# Combining short flags (with subcommand short flag) produces errors

---

_@ttytm_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

v4.0.2 (also wasn't working with v3, but with a less detailed error msg)

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand, ValueEnum};

#[derive(Parser)]
#[command(author, version, about, long_about = None)]
#[command(propagate_version = true)]
struct Cli {
	#[arg(value_parser)]
	pub name: Option<String>,

	#[command(subcommand)]
	command: Option<Commands>,
}

#[derive(Subcommand)]
enum Commands {
	/// Adds files to myapp
	#[clap(short_flag = 'a')]
	Add {
		/// Filename
		#[arg(short, value_parser)]
		option: Option<Options>,

		/// Subcommand
		#[command(subcommand)]
		subcommand: Option<Subcommands>,
	},
}

#[derive(Debug, PartialEq, Clone, Copy, ValueEnum)]
pub enum Options {
	A,
	B,
	C,
}

#[derive(Subcommand)]
enum Subcommands {
	#[clap(short_flag = 'c')]
	Config {
		/// Save the supplied values as default
		#[arg(short, value_parser, action)]
		save: bool,
		/// Wipe wthrr's configuration data
		#[arg(short, value_parser, action)]
		reset: bool,
	},
}

fn main() {
	let cli = Cli::parse();

	println!("Name arg was passed {:?}: ", cli.name);
	// You can check for the existence of subcommands, and if found use their
	// matches just as you would the top level cmd
	match &cli.command {
		Some(Commands::Add { option, .. }) => {
			println!("'add' was used, add option is {:?}", option)
		}
		_ => (),
	}
}
```

### Steps to reproduce the bug with the above code

Provide multiple short_flags to reproduce the problem:
E.g., this **won't** work:
`cargo run somename -aoa -cs`

Seperating into the shorthand values **will** work, e.g.,:
works:
`cargo run somename -aoa -c -s`

also works:
`cargo run somename -a -oa -cs`

### Actual Behaviour

Error:
```
 right: `Ok(())`: tracking of `flag_subcmd_skip` is off for `ShortFlags { inner: RawOsStr("cs"), utf8_prefix: CharIndices { front_offset: 2, iter: Chars([]) }, invalid_suffix: None }`', /home/turiiya/.cargo/registry/src/github.co
m-1ecc6299db9ec823/clap-4.0.0/src/parser/parser.rs:883:9
```

### Expected Behaviour

Providing multiple (unseparated) shorthand values e.g., `cargo run somename -aoa -cs` will work.

### Additional Context

_No response_

### Debug Output

    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-pacman somename -aoa -cs`
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="clap-pacman"
[      clap::builder::command]  Command::_propagate:clap-pacman
[      clap::builder::command]  Command::_check_help_and_version:clap-pacman expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_check_help_and_version: Building help subcommand
[      clap::builder::command]  Command::_propagate_global_args:clap-pacman
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:name
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("somename")' ([115, 111, 109, 101, 110, 97, 109, 101])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("somename")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-aoa")' ([45, 97, 111, 97])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("-aoa")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("aoa"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['a', 'o', 'a']) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_short_arg:iter:a
[        clap::parser::parser]  Parser::parse_short_arg:iter:a: subcommand=add
[        clap::parser::parser]  Parser::resolve_pending: id="name"
[        clap::parser::parser]  Parser::react action=Set, identifier=Some(Index), source=CommandLine
[        clap::parser::parser]  Parser::remove_overrides: id="name"
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="name", source=CommandLine
[      clap::builder::command]  Command::groups_for_arg: id="name"
[        clap::parser::parser]  Parser::push_arg_values: ["somename"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command]  Command::groups_for_arg: id="name"
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=name, pending=0
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: expected=1, actual=0
[        clap::parser::parser]  Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser]  Parser::parse_short_arg: cur_idx:=2
[        clap::parser::parser]  Parser::get_matches_with: After parse_short_arg FlagSubCommand("add")
[        clap::parser::parser]  Parser::get_matches_with:FlagSubCommandShort: subcmd_name=add, keep_state=true, flag_subcmd_skip=1
[        clap::parser::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[      clap::builder::command]  Command::_build_subcommand Setting bin_name of add to "clap-pacman add"
[      clap::builder::command]  Command::_build_subcommand Setting display_name of add to "clap-pacman-add"
[      clap::builder::command]  Command::_build: name="add"
[      clap::builder::command]  Command::_propagate:add
[      clap::builder::command]  Command::_check_help_and_version:add expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_check_help_and_version: Building help subcommand
[      clap::builder::command]  Command::_propagate_global_args:add
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:option
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::parse_subcommand: About to parse sc=add
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-aoa")' ([45, 97, 111, 97])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("-aoa")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("aoa"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['a', 'o', 'a']) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_short_arg:iter:o
[        clap::parser::parser]  Parser::parse_short_arg:iter:o: Found valid opt or flag
[        clap::parser::parser]  Parser::parse_short_arg:iter:o: val=RawOsStr("a") (bytes), val=[97] (ascii), short_arg=ShortFlags { inner: RawOsStr("aoa"), utf8_prefix: CharIndices { front_offset: 2, iter: Chars(['a']) }, invalid
_suffix: None }
[        clap::parser::parser]  Parser::parse_opt_value; arg=option, val=Some(RawOsStr("a")), has_eq=false
[        clap::parser::parser]  Parser::parse_opt_value; arg.settings=ArgFlags(NO_OP)
[        clap::parser::parser]  Parser::parse_opt_value; Checking for val...
[        clap::parser::parser]  Parser::react action=Set, identifier=Some(Short), source=CommandLine
[        clap::parser::parser]  Parser::react: cur_idx:=3
[        clap::parser::parser]  Parser::remove_overrides: id="option"
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="option", source=CommandLine
[      clap::builder::command]  Command::groups_for_arg: id="option"
[        clap::parser::parser]  Parser::push_arg_values: ["a"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[      clap::builder::command]  Command::groups_for_arg: id="option"
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=option, pending=0
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: expected=1, actual=0
[        clap::parser::parser]  Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser]  Parser::get_matches_with: After parse_short_arg ValuesDone
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-cs")' ([45, 99, 115])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("-cs")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("cs"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['c', 's']) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_short_arg:iter:c
[        clap::parser::parser]  Parser::parse_short_arg:iter:c: subcommand=config
[        clap::parser::parser]  Parser::parse_short_arg: cur_idx:=5
[        clap::parser::parser]  Parser::get_matches_with: After parse_short_arg FlagSubCommand("config")
[        clap::parser::parser]  Parser::get_matches_with:FlagSubCommandShort: subcmd_name=config, keep_state=true, flag_subcmd_skip=4
[        clap::parser::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[      clap::builder::command]  Command::_build_subcommand Setting bin_name of config to "clap-pacman add config"
[      clap::builder::command]  Command::_build_subcommand Setting display_name of config to "clap-pacman-add-config"
[      clap::builder::command]  Command::_build: name="config"
[      clap::builder::command]  Command::_propagate:config
[      clap::builder::command]  Command::_check_help_and_version:config expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:config
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:save
[clap::builder::debug_asserts]  Arg::_debug_asserts:reset
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::parse_subcommand: About to parse sc=config
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-cs")' ([45, 99, 115])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("-cs")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("cs"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['c', 's']) }, invalid_suffix: None }
thread 'main' panicked at 'assertion failed: `(left == right)`

---

_Label `C-bug` added by @ttytm on 2022-09-29 00:36_

---

_Renamed from "Short flags in a subcommands subcommand produce errors" to "Combining short flags produces errors" by @ttytm on 2022-09-29 00:37_

---

_Renamed from "Combining short flags produces errors" to "Combining short flags (with subcommand short flag) produces errors" by @epage on 2022-09-29 00:54_

---

_Comment by @epage on 2022-09-29 00:55_

I was worried at how our testing missed this until I saw subcommand short flags were involved and felt better when I also saw

> (also wasn't working with v3, but with a less detailed error msg)

---

_Label `A-parsing` added by @epage on 2022-09-29 00:55_

---

_Label `E-medium` added by @epage on 2022-09-29 00:55_

---

---
number: 3030
title: Optional option-argument parsing is incorrect
type: issue
state: open
author: mkayaalp
labels:
  - C-bug
  - A-parsing
  - S-triage
assignees: []
created_at: 2021-11-15T19:57:10Z
updated_at: 2025-11-27T15:52:31Z
url: https://github.com/clap-rs/clap/issues/3030
synced_at: 2026-01-07T13:12:19-06:00
---

# Optional option-argument parsing is incorrect

---

_Issue opened by @mkayaalp on 2021-11-15 19:57_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0 (09c42c458 2021-10-18)

### Clap Version

master

### Minimal reproducible code

```rust
use clap::Parser;
#[derive(Parser, Debug)]
struct Opt {
    #[clap(short,long)]
    foo: Option<Option<String>>,
    input: String,
}

fn main() {
    let opt = Opt::parse();
    println!("{:?}", opt);
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- -f input
```
or
```
cargo run -- --foo input
```

### Actual Behaviour

```
error: The following required arguments were not provided:
    <INPUT>

USAGE:
    clap-test --foo <FOO> <INPUT>

For more information try --help
```

### Expected Behaviour

```
Opt { foo: Some(None), input: "input" }
```

### Additional Context

There needs to be some special handling of options with optional arguments. 

#### Optional option-arguments are bad

First of all, users really need to be discouraged (if we ever add some lint-like feature, e.g. `#[clap(enable-warnings)`). 

[POSIX](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) says:

>Option-arguments should not be optional.

[Solaris](https://docs.oracle.com/cd/E23824_01/html/821-1461/intro-1.html) says:

> Option-arguments cannot be optional.

#### Parsing

I know some people have a preference for `-f OptArg` over `-fOptArg`, but if the option-argument is optional, then only the latter is correct. If there is a space, it means the optional argument is not there.

From [POSIX.1-2017 - Ch. 12: Utility Conventions](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html):
> a conforming application shall place any option-argument for that option directly adjacent to the option in the same argument string, without intervening `<blank>` characters

> If the utility receives an argument containing only the option, it shall behave as specified in its description for an omitted option-argument; it shall not treat the next argument (if any) as the option-argument for that option.

Same thing with the long option, which is a GNU extension.

From [GNU C Library Reference Manual - Ch. 25.1.1: Program Argument Syntax Conventions](https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html#Argument-Syntax):

> To specify an argument for a long option, write `--name=value`. This syntax enables a long option to accept an argument that is itself optional.

See this SO question about how `getopt` handles this: [`getopt` does not parse optional arguments to parameters](https://stackoverflow.com/questions/1052746/getopt-does-not-parse-optional-arguments-to-parameters).

In clap terms, these are saying we should only allow
- `--long`
- `--long=value`
- `-s`
- `-svalue`

In contrast, clap supports
- Default:
  - `--long`
  - `--long=value`
  - `--long value`
  - `-s`
  - `-s value`
  - `-s=value`
  - `-svalue`
- `requires_equals(true)`
  - `--long`
  - `--long=value`
  - `-s`
  - `-s=value`

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:clap-test
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:clap-test
[            clap::build::app] 	App::_derive_display_order:clap-test
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:foo
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:input
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-f")' ([45, 102])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-f")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: short_arg=RawOsStr("f")
[         clap::parse::parser] 	Parser::parse_short_arg:iter:f
[         clap::parse::parser] 	Parser::parse_short_arg:iter:f: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_short_arg:iter:f: Found valid opt or flag
[         clap::parse::parser] 	Parser::parse_short_arg:iter:f: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=RawOsStr("f")
[         clap::parse::parser] 	Parser::parse_opt; opt=foo, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of: arg=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg Opt(foo)
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("input")' ([105, 110, 112, 117, 116])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=foo, val=RawOsStr("input")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."input"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app] 	App::groups_for_arg: id=foo
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=foo
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: max_vals...1
[         clap::parse::parser] 	Parser::remove_overrides
[         clap::parse::parser] 	Parser::remove_overrides:iter:foo
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_env: Checking arg `--help`
[         clap::parse::parser] 	Parser::add_env: Skipping existing arg `--foo <FOO>`
[         clap::parse::parser] 	Parser::add_env: Checking arg `<INPUT>`
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:foo:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:foo: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:foo: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:input:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:input: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:input: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:foo
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::gather_conflicts:iter: id=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([Child { id: input, children: [] }])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:foo
[      clap::parse::validator] 	Validator::validate_required:iter:aog=input
[      clap::parse::validator] 	Validator::validate_required:iter: This is an arg
[      clap::parse::validator] 	Validator::is_missing_required_ok: input
[      clap::parse::validator] 	Validator::validate_arg_conflicts: a="input"
[      clap::parse::validator] 	Validator::missing_required_error; incl=[]
[      clap::parse::validator] 	Validator::missing_required_error: reqs=ChildGraph([Child { id: input, children: [] }])
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={input}
[         clap::output::usage] 	Usage::get_required_usage_from:iter:input
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["<INPUT>"]
[      clap::parse::validator] 	Validator::missing_required_error: req_args=[
    "<INPUT>",
]
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[foo], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={input}
[         clap::output::usage] 	Usage::get_required_usage_from:iter:foo
[         clap::output::usage] 	Usage::get_required_usage_from:iter:input
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["--foo <FOO>", "<INPUT>"]
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
[           clap::output::fmt] 	is_a_tty: stderr=true
```

---

_Label `T: bug` added by @mkayaalp on 2021-11-15 19:57_

---

_Referenced in [clap-rs/clap#2468](../../clap-rs/clap/issues/2468.md) on 2021-11-15 20:00_

---

_Comment by @epage on 2021-11-15 20:05_

Would the summary be that we for optional values, we should be encouraging `requires_delimiter` (granted, I've not checked what that does in the short flag case or if that deviates from these standards)?

---

_Comment by @mkayaalp on 2021-11-15 20:36_

That changes the parsing for `multiple_values`, right? Couldn't test that with `clap_derive` but, the following gives the same error (I tried to adapt the `cargo expand` output to use the builder pattern):
```rust
  let opt = App::new("clap-test")
    .arg(Arg::new("foo")
        .short('f')
        .long("foo")
        .takes_value(true)
        .value_name("FOO")
        .min_values(0)
        .max_values(1)
        .multiple_values(false)
        .use_delimiter(true)
        .require_delimiter(true))
    .arg(Arg::new("input")
        .takes_value(true)
        .value_name("INPUT")
        .required(true)).get_matches();
```

The POSIX guideline says:
> When multiple option-arguments are specified to follow a single option, they should be presented as a single argument, using `<comma>` characters within that argument or `<blank>` characters within that argument to separate them.

So, `-O a,b,c` or `-O "a b c"`

Solaris guideline says:
> Groups of option-arguments following an option must either be separated by commas or separated by tab or space character and quoted (`-o xxx,z,yy` or `-o"xxx z yy"`).

I don't know if the lack of space between option and quote in `-o"xxx z yy"` is a typo, but if the option-arguments were optional, then that would be correct (instead of `-o "xxx z yy"`).

---

_Comment by @mkayaalp on 2021-11-15 20:48_

This could be a separate issue, but the synopsis for optional option-argument should be:
```
USAGE:
    clap-test [--foo[=FOO]] <INPUT>
```
or 
```
USAGE:
    clap-test [-f[FOO]] <INPUT>
```

---

_Comment by @epage on 2021-11-15 20:50_

Sorry, I mean to recommend `requires_equals`.  `requires_delimiter` is for the other case you mentioned.

---

_Comment by @epage on 2021-11-15 20:50_

(too many generic names,. keep referring to the wrong one. #3026 talks about cleaning this up)

---

_Comment by @mkayaalp on 2021-11-15 20:58_

> Sorry, I mean to recommend `requires_equals`

Yes. That works for long, but it also requires it for short: `-f` works (no arg) but `-fval` does not work anymore. It requires `-f=val`.

---

_Comment by @epage on 2021-11-15 21:07_

Yeah, I was worried it didn't do the right thing in that case.

---

_Comment by @epage on 2021-11-15 21:11_

I tried to summarize this at the bottom of your "Parsing" section.  Mind double checking it?

---

_Comment by @epage on 2021-11-15 21:22_

Random observations:

1 atm the color flag in my CLIs is [`--color=<WHEN>`](https://docs.rs/concolor-clap/0.0.6/src/concolor_clap/lib.rs.html#31-35) when ideally it would be `--color[=<WHEN>]`.  One of the things that has held me back from this is I've been unsure how users would react to the inconsistency of some args requiring `=` while others do not, depending on whether they have flag/option duality or just an option.  For the user,. when thinking about it as an option, they are likely to think of it like any other option.

2 For the most part, users are constructing the optional-optional value, we are just providing the building blocks. In following that pattern, we'd need to be able to allow users to specify the policy in a way that isn't confusing.  So how do we communicate this?  One thought is to have an enum listing out delimited (`=`), arg (` `), or concatenate (`-svalue`) and the user can provide a set of these to a long and short function.  This requires the user to build it up how they want.  We'd want to document what the recommendations would be.  We could have `clap_derive` do "the right thing" for `Option<Option<T:>>`.

My main concern about this is the overall complexity, not just in code but in API design and user code.  Is there a simpler way?

---

_Comment by @mkayaalp on 2021-11-15 21:26_

> I tried to summarize this at the bottom of your "Parsing" section. Mind double checking it?

That's correct. Although I don't know if I would say `clap` *supports* `-s` and `-s value` when it interprets the operand (positional argument) to be the option-argument and gives an error.

---

_Comment by @mkayaalp on 2021-11-15 21:43_

> atm the color flag in my CLIs is [`--color=<WHEN>`](https://docs.rs/concolor-clap/0.0.6/src/concolor_clap/lib.rs.html#31-35) when ideally it would be `--color[=<WHEN>]`

I would say `--color[=WHEN]`.

> For the user, when thinking about it as an option, they are likely to think of it like any other option.

But `--name=value` *IS* the correct form. Re-quoting GNU:

> To specify an argument for a long option, write --name=value.

Allowing a space is the parser being lenient. 

Same thing with `-s value` being the norm for short options and `-svalue` being allowed for leniency. Except for optional option-arguments. Then, it's the other way around. Solaris did have a good reason to outlaw those, I think. 


---

_Comment by @epage on 2021-11-15 21:51_

> Allowing a space is the parser being lenient.

While GNU is saying one thing, we still have to deal with user expectations.  As a user, I never use `=` and I expect many others do the same.  It doesn't matter what standard I reference, the user will still be frustrated at the inconsistency.

---

_Comment by @mkayaalp on 2021-11-15 21:52_

> we'd need to be able to allow users to specify the policy in a way that isn't confusing.

We could say "short" option follows the POSIX guidelines, and "long" option follows the GNU guidelines. Then have a custom option escape hatch that changes the prefix, separator, delimiter, with allow/disallow/require flags for each. 

---

_Comment by @mkayaalp on 2021-11-15 22:00_

> we still have to deal with user expectations.

I agree, I am not saying we make the parser strict (although, it would be a really good addition to `AppSettings`).

As a user, I am expecting `--foo input` to parse it as option `foo` without argument and `input` as the operand. And I would also expect `--foo optarg input` to parse as option `foo` with `optarg` as the argument and `input` as the operand (which could very well fail, as it is easily ambiguous: if `input` was optional or `variadic`).

But I would prefer to show `[--foo[=OPTARG]]` in the usage as the canonical form.

---

_Comment by @mkayaalp on 2021-11-15 22:28_

> For the most part, users are constructing the optional-optional value, we are just providing the building blocks.

Well, we do have a short/long/operand distinction. If we had simpler building blocks, short/long options could be "non-positional optional operands with labels" and short and long could only differ in their label length and prefix (`-`/`--`) and alias each other. We would need to have a combining/stacking feature for short options to use.

Going the other way, maybe we need another distinction for optional option-arguments: `short_optional_arg` and `long_optional_arg`. We could make sure the type was suitable for `clap_derive`: `Option<Option<T>>` or `Option<Vec<T>>`.

---

_Referenced in [epage/clapng#241](../../epage/clapng/issues/241.md) on 2021-12-06 22:26_

---

_Label `S-triage` added by @epage on 2021-12-13 22:13_

---

_Label `A-parsing` added by @epage on 2021-12-13 22:41_

---

_Referenced in [clap-rs/clap#4499](../../clap-rs/clap/issues/4499.md) on 2022-11-22 18:07_

---

_Referenced in [clap-rs/clap#4702](../../clap-rs/clap/issues/4702.md) on 2023-02-13 16:03_

---

_Comment by @mina86 on 2023-02-13 17:21_

> While GNU is saying one thing, we still have to deal with user expectations.

Many users are used to GNU and Unix-like systems their expectation is that flags will behave the way GNU and POSIX describes.  Users aren’t dumb.  They can understand CLI conventions so long as they are somewhat consistent between applications.  It’s when every application has their own idea of how arguments should be parsed is when we get into trouble.  (And I’m sorry to say but clap is contributing to that problem).

---

_Comment by @epage on 2023-02-13 17:56_

>  (And I’m sorry to say but clap is contributing to that problem).

Could you clarify which problem clap is contributing to?  If its allowing a space between a flag and a value, the cat is out of the bag on that one and its not just clap.  If its for something else, please clarify,

---

_Comment by @mina86 on 2023-02-13 18:36_

clap is definitely not the only offender, but it does contribute to the problem.

The reason I have irrational hatred of clap are:
* Flags with optional value should require an equal sign when using long name and be part of the argument when using a short flag.
* Equal sign with short flags should not be a thing.  `-f=val` should just be flag `f` with value `=val`.  This also makes `require_equals(true)` which would somewhat solve the previous issue completely useless.
* `Arg::num_args` should not be a thing.

And yes, I’m aware cat is out of the bag and I’ll have to continue dealing with my irrational hatred but perhaps some of clap parsing may be made saner.


---

_Comment by @epage on 2023-02-13 19:36_

>  perhaps some of clap parsing may be made saner.

I regret to inform you, it likely isn't.  Looking at the high level options for making the ecosystem "saner":
- Exclusively changing to what you propose:  This is off the table as that would be a breaking change to fundamental users of clap like the rust project.
- Pushing people to your proposal via defaults, with configuration to roll back to old behavior for compatibility:
  - A strong case is needed for why we should choose to match GNU/POSIX instead of the rest of the modern CLI ecosystem:
    - Python's argparse accepts it, covering a large swath of user and corporate scripting
    - Judging by `podman --help`, it looks like cobra accepts it which covers a lot of the new container tools
    - clap accepts it which covers rust-lang and a large swath of the "for human" CLI tools like ripgrep
    - anecdote: when I see stricter CLI parsers, the general sentiment from developer comments is not in praising them for doing so but disappointment, only using it if there isn't a better alternative
  - I expect rust-lang, ripgrep,. and many of the tools that might frustrate you would choose the old behavior, either due to compatibility or because its what they are aiming for in behavior, reducing the gains from changing the default and making the case for changing the default harder
  - This presupposes configurability which tends to run counter to our current goals are to shrink binary size, reduce compile times, and improve API discoverability, re-inforcing why we need a strong case to change the defaults.

For allowing individual apps to be "saner", we'll need a strong case for why or where being more permissive is a problem  due to the aforementioned challenges with configurability.  Design work on this would also need to come from the wider community as this is not one of our stated focus areas. I'm most sympathetic to cases like uutils where they are trying to match behavior but even then I would need to understand why being more lax is a problem so long as all of the strict cases work.

---

_Referenced in [clap-rs/clap#5489](../../clap-rs/clap/pulls/5489.md) on 2024-05-09 04:32_

---

_Comment by @my4ng on 2024-05-09 10:01_

I have proposed a solution in PR #5489 where `require_equals` is limited to long options and a new setting `require_no_space` only allows the `-oval` format for short options. With both set, this should be compliant to the POSIX/GNU conventions for optional option-argument, for example:

```
Usage: myapp [OPTIONS]

Options:
  -f[<foo>], --foo[=<foo>]  This is foo
  -h, --help                Print help
```

---

_Referenced in [uutils/coreutils#6387](../../uutils/coreutils/issues/6387.md) on 2024-05-09 11:23_

---

_Referenced in [uutils/coreutils#6962](../../uutils/coreutils/issues/6962.md) on 2024-12-27 00:29_

---

_Referenced in [uutils/findutils#594](../../uutils/findutils/issues/594.md) on 2025-11-19 22:40_

---

_Comment by @brian-pane on 2025-11-27 15:52_

Would it make sense to keep the default behavior the same and add an `allow_space` override function, like this?
```
// Interpret "-abc" as -a with option value "bc" and "-a bc" as -a with no value followed by positional parameter "bc"
Arg::new("a")
    .short('a')
    .num_args(0..=1)
    .require_equals(false)
    .allow_space(false)
```


---

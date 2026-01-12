```yaml
number: 1553
title: "Multiple subcommand categories / `help_heading`s"
type: issue
state: open
author: BrettMayson
labels:
  - C-enhancement
  - M-breaking-change
  - A-help
  - E-easy
assignees: []
created_at: 2019-09-27T18:01:53Z
updated_at: 2025-11-14T21:51:10Z
url: https://github.com/clap-rs/clap/issues/1553
synced_at: 2026-01-12T16:14:11Z
```

# Multiple subcommand categories / `help_heading`s

---

_@BrettMayson_

### Feature Request Summary
It would be nice to be able to have multiple subcommand groupings. 

### Sample
An example of what I mean can be found at https://github.com/synixebrett/HEMTT/issues/195


---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Label `good first issue` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 12:10_

---

_Referenced in [TeXitoi/structopt#392](../../TeXitoi/structopt/issues/392.md) on 2020-05-15 06:06_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#126](../../epage/clapng/issues/126.md) on 2021-12-06 18:37_

---

_Comment by @epage on 2021-12-08 20:07_

@pksunkara we were just talking about whether `App::subcommand_help_heading` should be supported in a way to allow individual subcommands to have their own help heading like args.

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:07_

---

_Comment by @pksunkara on 2021-12-09 08:44_

Looks like I didn't need to create an issue for what we were talking about.

---

_Comment by @epage on 2021-12-09 12:26_

The question we need to resolve with this is how to handle naming

`App::help_heading` applies to future args

`App::subcommand_help_heading` globally applies to subcommands as if you called `App::help_heading` at the start and never called `Arg::help_heading`.

Maybe if we changed the behavior so that
- `App::help_heading` applied to future args **and subcommands**
- `App::subcommand_help_heading` applied to the current subcommand like `Arg::subcommand`
  - We'd have to deal with what to name `App::subcommand_value_name` because that was mean to be parallel to the current behavior

This would be a breaking change and one that doesn't offer a transition path with deprecations.  Any other ideas for how we could do this?

---

_Renamed from "Multiple subcommand categories" to "Multiple subcommand categories / `help_heading`s" by @epage on 2021-12-09 20:18_

---

_Referenced in [clap-rs/clap#2289](../../clap-rs/clap/issues/2289.md) on 2021-12-09 20:18_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 20:19_

---

_Label `E-help-wanted` removed by @epage on 2021-12-09 20:19_

---

_Label `E-easy` removed by @epage on 2021-12-09 20:19_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 20:19_

---

_Referenced in [clap-rs/clap#1807](../../clap-rs/clap/issues/1807.md) on 2021-12-09 22:33_

---

_Comment by @pksunkara on 2021-12-10 14:03_

Bikeshedding on the name to make it more clear, 

* `App::help_heading` => `Command::help_heading_group`
* `App::subcommand_help_heading` => `Command::help_heading` (similar to `Arg::help_heading`)

One concern with combining the arg and app help heading setting in one function is what to do when they have a common heading? How do we display the help?

---

_Comment by @epage on 2021-12-10 17:27_

One option I had been considering that I forgot until your post is `Command::next_help_heading`.

I worry people will just refer to what help headings as groups or categories, so using a `_group` suffix I don't think will convey the intent of "all following are being grouped under the this heading" (which I'm assuming is the intent of the suggestion).

---

_Comment by @pksunkara on 2021-12-10 17:30_

Yeah, I think I am trying to convey that the function sets help heading for **both** args and subcommands. Maybe it's better if we just divide them into `args_help_heading` and `subcommands_help_heading`. But I think `next_help_heading` is better than `help_heading_group` definitely.

---

_Comment by @epage on 2021-12-10 17:42_

So to summarize the solution:
- `App::help_heading` would
  - Be renamed `App::next_help_heading`
  - Will also apply to subcommands
- `App::subcommand_help_heading` would be removed
- `App::help_heading` would be added and acts like `Arg::help_heading` but for subcommands
- We would need to add tests to make sure `next_help_heading` on a `derive(Subcommand)` behaves as expected.

I guess the downside to the name `next_help_heading` is in derives.  This is also getting me thinking that maybe we should provide some more flexibility by renaming our attribute from `clap` to `clap::app` and `clap::arg`.  This would allow a user to set app settings in the middle of the derive.  If that idea sounds like it has potential, I'll create a separate issue for further discussion.

---

_Label `M-breaking-change` added by @pksunkara on 2021-12-10 17:44_

---

_Referenced in [clap-rs/clap#3165](../../clap-rs/clap/issues/3165.md) on 2021-12-13 19:29_

---

_Added to milestone `4.0` by @epage on 2021-12-14 15:26_

---

_Comment by @epage on 2022-02-08 01:23_

#3414 changed `App::help_heading` -> `App::next_help_heading`.  The rest of the work is reserved for clap 4.

---

_Referenced in [clap-rs/clap#3444](../../clap-rs/clap/pulls/3444.md) on 2022-02-11 01:33_

---

_Referenced in [clap-rs/clap#3482](../../clap-rs/clap/issues/3482.md) on 2022-02-17 15:06_

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-08-27 02:18_

---

_Comment by @epage on 2022-09-07 15:29_

Think I'm going to push this out.

In working on this, I realized a problem we'd have is how to set the help heading for the implicit help subcommand and help/version flags.

---

_Removed from milestone `4.0` by @epage on 2022-09-07 15:29_

---

_Added to milestone `5.0` by @epage on 2022-09-07 15:29_

---

_Referenced in [LGUG2Z/komorebi#345](../../LGUG2Z/komorebi/issues/345.md) on 2023-02-12 18:58_

---

_Referenced in [jj-vcs/jj#1344](../../jj-vcs/jj/pulls/1344.md) on 2023-03-07 04:38_

---

_Referenced in [clap-rs/clap#2222](../../clap-rs/clap/issues/2222.md) on 2023-03-30 18:56_

---

_Referenced in [Yamato-Security/hayabusa#991](../../Yamato-Security/hayabusa/issues/991.md) on 2023-04-12 12:10_

---

_Referenced in [Yamato-Security/hayabusa#1001](../../Yamato-Security/hayabusa/issues/1001.md) on 2023-04-13 01:36_

---

_Referenced in [wasmerio/wasmer#3863](../../wasmerio/wasmer/issues/3863.md) on 2023-05-15 14:30_

---

_Referenced in [clap-rs/clap#5098](../../clap-rs/clap/issues/5098.md) on 2023-08-28 18:46_

---

_Comment by @milesj on 2023-09-02 18:47_

Been a year now, has there been any movement on this?

---

_Comment by @YamatoSecurity on 2024-03-03 20:58_

We would love this feature as well... only being able to have one group for commands is very limiting and makes for a bad UI for the user.

---

_Referenced in [embik/kubeconfig-bikeshed#56](../../embik/kubeconfig-bikeshed/issues/56.md) on 2024-05-15 15:09_

---

_Referenced in [astral-sh/uv#5702](../../astral-sh/uv/issues/5702.md) on 2024-08-03 15:13_

---

_Comment by @Fiedzia on 2024-11-08 00:12_

I am trying to implement it. I am wondering where to place command that do not belong to any groups and the "commands" heading.
Option 1: commands not belonging to any group go to top
```
Commands:
cmd1 ...
cmd2 ...

Group1:
cmd3
cmd4

Group2:
cmd5
cmd6
```

meaning placing commands not belonging to any group on top.
Alternatively, They could go to bottom. This makes more sense to me,
but maybe not to other people.

```
Group1:
cmd3
cmd4

Group2:
cmd5
cmd6

Commands:
cmd1
cmd2

```
and what to do when heading is provided, but all commands are grouped:

```
Commands:

Group1:
cmd3
cmd4
```

Any thoughts?

---

_Comment by @epage on 2024-11-08 18:56_

I'd recommend doing what we do for Options.

However, before going to far down this, how do you plan to handle the `help` subcommand?  That is why I stopped this work before.  In clap v3, we had a bunch of specific helpers for overriding builtin arguments (`--help`, `--version`) and we simplified things a lot (improving performance and reducing binary bloat) by only supporting users disabling the built-in argument and providing it themself.  Currently, there isn't a way to do the same thing with the `help` subcommand.  My assumption is that we need to solve that problem first before we add custom help headings for subcommands.

---

_Comment by @Fiedzia on 2024-11-08 22:30_

Currently a CommandGroup would be added, and it can have optional heading (group won't show a name for group unless you specify heading).

---

_Comment by @epage on 2024-11-08 22:34_

I would like this to be consistent with arguments.  If there is a reason we should change arguments, we should have that  as a separate conversation.

That said, that doesn't say how users can use this with a built-in command (`help`).  As I said, there is likely something we need to solve there first.

---

_Comment by @Fiedzia on 2024-11-08 23:07_

> I would like this to be consistent with arguments.

Ok, makes sense. Arguments print heading and a list of ungrouped arguments on top, no heading if there are no ungrouped arguments.

> that doesn't say how users can use this with a built-in command (help)

With current implementation I have, it treats it as any other command. By default it is ungrouped. You can put it into a group if you wish (well, you can via builder, not sure about derive). Seems intuitive to me.


---

_Comment by @epage on 2024-11-09 02:11_

> Ok, makes sense. Arguments print heading and a list of ungrouped arguments on top, no heading if there are no ungrouped arguments.

Also, you specify a help heading.  Argument groups are for associations and not visualization.

> With current implementation I have, it treats it as any other command. By default it is ungrouped. You can put it into a group if you wish (well, you can via builder, not sure about derive). Seems intuitive to me.

How do you put `help` under a custom heading?

---

_Comment by @Fiedzia on 2024-11-13 06:27_

WIP, but if anyone is interested: https://github.com/Fiedzia/clap/tree/command-groups

---

_Referenced in [clap-rs/clap#5815](../../clap-rs/clap/issues/5815.md) on 2024-11-13 22:23_

---

_Referenced in [clap-rs/clap#5816](../../clap-rs/clap/pulls/5816.md) on 2024-11-14 01:46_

---

_Referenced in [clap-rs/clap#5818](../../clap-rs/clap/pulls/5818.md) on 2024-11-14 22:49_

---

_Referenced in [clap-rs/clap#5819](../../clap-rs/clap/pulls/5819.md) on 2024-11-14 23:22_

---

_Referenced in [clap-rs/clap#5828](../../clap-rs/clap/issues/5828.md) on 2024-11-26 16:57_

---

_Referenced in [clap-rs/clap#6061](../../clap-rs/clap/issues/6061.md) on 2025-07-07 14:38_

---

_Referenced in [autobib/autobib#232](../../autobib/autobib/issues/232.md) on 2025-10-07 10:32_

---

_Comment by @jerusdp on 2025-11-02 22:33_

I have an application for the work complete by @Fiedzia  and would like to see this feature incorporated into clap. @epage  has drawn my attention to the status of the change `waiting-on-design`. And so I will start by stating the re-stating the design as I understand it from this tread and addressing the outstanding question of what to do with the help command. 

Expecting comment to follow 😄 

## Design statements:
-  MUST provide that a custom section header can be specified for a command
-  MUST display all commands with the same section header together under that section header in help
-  MUST display any commands without a section header and auto-generated commands (e.g. help) first in the commands section
- WILL NOT provide a facility to specify a section header for auto-generated commands such as help

## Examples of expected output v derive input

 ### Output - user commands in sections

```
Commands:
  help      Print this message or the help of the given subcommand(s)
  test1     Command without a help section

First test section commands:
  test2    Command in first test section

Second test section commands:
  test3    Command in second test section
```

 ### Derive Specification - user commands in sections

```rust
#[derive(Subcommand, Debug)]
enum SubCmds {
     ///  Command without a help section
    #[clap(name = "test1")]
    test1, 

     ///  Command in first test section
    #[clap(name = "test2", subcommand_help_heading = "First test section commands")]
    test2, 

     ///  Command in second test section
    #[clap(name = "test3", subcommand_help_heading = "Second test section commands") )]
    test3, 
}
```

 ### Output - user commands with amended command title

```
Custom Commands:
  help      Print this message or the help of the given subcommand(s)
  test1     Command without a help section

First test section commands:
  test2    Command in first test section

Second test section commands:
  test3    Command in second test section
```

 ### Derive Specification - user commands in sections

```rust
#[derive(Subcommand, Debug)]
#[clap(subcommand_help_heading = "Custom Commands")
enum SubCmds {
     ///  Command without a help section
    #[clap(name = "test1")]
    test1, 

     ///  Command in first test section
    #[clap(name = "test2", subcommand_help_heading = "First test section commands")]
    test2, 

     ///  Command in second test section
    #[clap(name = "test3", subcommand_help_heading = "Second test section commands") )]
    test3, 
}
```

---

_Comment by @epage on 2025-11-03 22:12_

> WILL NOT provide a facility to specify a section header for auto-generated commands such as help

I was specifically asking in this thread how we can support this and your statement is that we won't.  Can you provide justification as to why?

---

_Comment by @jerusdp on 2025-11-04 17:33_

> I was specifically asking in this thread how we can support this and your statement is that we won't. Can you provide justification as to why?

The reason was that with the help remaining under the original Command title if it was required that heading could be set using `subcommand_help_heading`. 

---

_Comment by @epage on 2025-11-04 17:39_

So to say a different way, your proposal for solving the issue for `help` is to tell people to use `subcommand_help_heading`?

---

_Comment by @jerusdp on 2025-11-05 07:57_

That would be a yes. However, reflecting on it, it does not meet the case where a user wants to place a heading on the help command only and leave the other commands under the default heading. I also sense that for you as a design statement its a MUST. 

The implementation would require an API to capture the custom heading, say `custom_help_command_heading`. 

## Revised design statements: 

- MUST provide that a custom section header can be specified for a command
- MUST display all commands with the same section header together under that section header in help
- MUST display any commands without a section header and auto-generated commands (e.g. help) first in the commands section
- MUST provide a facility to specify a section header for auto-generated commands such as help

Additional example to those [here](https://github.com/clap-rs/clap/issues/1553#issuecomment-3478440572): 


## Output - custom help section

```sh
Commands:
  test1     command under default heading

Custom help section:
  help      Print this message or the help of the given subcommand(s)
```

## Derive Specification - custom help section

```rust
#[derive(Subcommand, Debug)]
#[clap(custom_help_command_heading = "Custom help section")
enum SubCmds {
     ///  command under default heading
    #[clap(name = "test1")]
    test1, 
}
```


---

_Comment by @epage on 2025-11-05 17:46_

So let me step back and step through the initial proposal: https://github.com/clap-rs/clap/issues/1553#issuecomment-991167908

We add a `Command::help_heading` that describes the current command's help heading, much like `Arg::help_heading`.

`Command::next_help_heading` would be changed to apply to subcommands, not just arguments.  This would be a breaking change and would need to wait until clap v5.

We planned for `Command::subcommand_help_heading` to be removed as redundant.  This would match `Arg` as there is no way to set a default for `Arg` help headings outside of `Command::next_help_heading` / `Arg::help_heading`.  As removal would be a breaking change, this would start as a deprecation.  Unsure whether that deprecation should wait for `Command::next_help_heading` being applied to subcommands.

So where does that leave `help`?  Let's look at precedence:  `--help` and `--version` are not subject to `next_help_header`
https://github.com/clap-rs/clap/blob/cb49ebad046317a6bef22a553f6f1c8b7852853c/clap_builder/src/builder/command.rs#L4817-L4819

So to override the help heading for `--help` and `--version`, you need to disable the built-in versions and provide your own.

So for parity, the ideal would be if you could define your own `help` command and get the same behavior.  At minimum, this would require we add `CommandAction`, like `ArgAction`.  Note that this would be an action that runs when a command is first encountered and not pluggable actions after a command has run which has been requested from time to time (e.g. [clap-nested](https://crates.io/crates/clap-nested)).  This is a larger effort with many unknowns to be worked out.

Maybe in the short term we could do as you said and could keep `Command::subcommand_help_heading` or replace it with `Command::custom_help_heading` (name TBD).  Instead of an all-or-nothing replacement of `--help`, we used to provide overrides for specific parts, like [`App::help_short`](https://docs.rs/clap/2.32.0/clap/struct.App.html#method.help_short).  So this would be us reverting back on our removing that approach.  Maybe that is ok as a hold over until the replacement becomes available?  If its short term and we already need to keep `Command::subcommand_help_heading` for a little bit, maybe its better to not create a new concept but just encourage people to use the soon-to-be deprecated function?

---

_Comment by @jerusdp on 2025-11-06 08:09_

I think we are making some progress. 

The design as I see it  has three main topics : 
- providing custom headings for user commands
- providing a custom heading for help
- breaking changes to plan for next release

## High Level Design

### Custom help headings 

Addressing: 
- MUST provide that a custom section header can be specified for a command
- MUST display all commands with the same section header together under that section header in help
- MUST display any commands without a section header and auto-generated commands (e.g. help) first in the commands section

Provide API `Command::help_heading` to set the current help heading and associated field (similar to `Arg::help_heading`).  

### Custom command heading for `help` command

Addressing:
- MUST provide a facility to specify a section header for auto-generated commands such as help

Create new API `command::custom_heading_for_help` rather than re-purposing `command::subcommand_help_heading`  as this will allow for the user to specify commands that they wish to specify under the default heading, custom headings and place `help` under a custom heading if desired. 

If the  user requires a different default command heading `command::subcommand_help_heading` can be used to set that heading and `help` can be specified under a different custom heading. 

### Changes deferred to new issue as breaking

Addressing:
- WILL NOT change scope of command::next_help_heading to include subcommands (issue for v5)
- COULD deprecate `command::subcommand_help_heading` in preparation for removal (issue for v5)
 
`command::next_help_heading` becomes a MUST to deliver consistency across subcommands and args. 
`command::subcommand_help_heading` becomes a MUST to deprecate in favour of `command::next_help_heading`

## Design Vision

This design allow the following comprehensive test to pass:

```rust
fn multiple_commands_mixed_standard_and_custom_headers() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Commands:
  def_cmd1  First command under default command heading
  def_cmd2  Second command under default command heading
  def_cmd3  Third command under default command heading
  def_cmd4  Fourth command under default command heading

First Custom:
  ch1_cmd1  First command under first custom command heading
  ch1_cmd2  Second command under first custom command heading
  ch1_cmd3  Third command under first custom command heading
  ch1_cmd4  Fourth command under first custom command heading

Second Custom:
  ch2_cmd1  First command under second custom command heading
  ch2_cmd2  Second command under second custom command heading
  ch2_cmd3  Third command under second custom command heading
  ch2_cmd4  Fourth command under second custom command heading

Help Section:
  help      Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Help Section")
        .subcommand(Command::new("def_cmd1").about("First command under default command heading"))
        .subcommand(Command::new("def_cmd2").about("Second command under default command heading"))
        .subcommand(Command::new("def_cmd3").about("Third command under default command heading"))
        .subcommand(Command::new("def_cmd4").about("Fourth command under default command heading"))
        .subcommand(
            Command::new("ch1_cmd1")
                .about("First command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch1_cmd2")
                .about("Second command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch1_cmd3")
                .about("Third command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch1_cmd4")
                .about("Fourth command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd1")
                .about("First command under second custom command heading")
                .help_heading("Second Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd2")
                .about("Second command under second custom command heading")
                .help_heading("Second Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd3")
                .about("Third command under second custom command heading")
                .help_heading("Second Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd4")
                .about("Fourth command under second custom command heading")
                .help_heading("Second Custom"),
        );

    utils::assert_output(cmd, "clap-test --help", VISIBLE_ALIAS_HELP, false);
}
```

---

_Comment by @epage on 2025-11-07 22:10_

> `Command::custom_heading_for_help`

In theory, this is short-lived.  In theory, `Command::subcommand_help_heading` is short lived.  I wonder if its worth having people migrate twice and if it would be 
better to just migrate once and just keep `Command::subcommand_help_heading` until we have the replacement.

---

_Comment by @jerusdp on 2025-11-08 09:13_

> > `Command::custom_heading_for_help`
> 
> In theory, this is short-lived. In theory, `Command::subcommand_help_heading` is short lived. I wonder if its worth having people migrate twice and if it would be better to just migrate once and just keep `Command::subcommand_help_heading` until we have the replacement.

I don't see it quite that way. 
Considering the following phases:
- Phase 1 - implementation of the designed referred to in this issue
- Phase 2 - implementation of deferred changes
- Phase 3 - Removal of API deprecated in Phase 2

In phase 2 `Command::subcommand_help_heading` is deprecated in favour `Command::next_heading`.  The `Command::custom_heading_for_help` API is retained allow the user to specify a separate heading for the help command while leaving other commands under the standard heading (or standard heading as modified by `Command::next_help_heading`). 


## Tests for Phase 1 

```rust
fn subcommand_with_different_custom_for_cmds_and_help() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Custom Commands:
  test  Some help

Custom Help:
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Custom Help")
        .subcommand_help_heading("Custom Commands")
        .subcommand(Command::new("test").about("Some help"));

    utils::assert_output(cmd, "clap-test --help", VISIBLE_ALIAS_HELP, false);
}

fn test_two_custom_headings_for_cmd_and_custom_heading_on_help() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Custom Commands:
  test  Some help

Costume Commands:
  show  Show costume

Custom Help:
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Custom Help")
        .subcommand_help_heading("Custom Commands")
        .subcommand(Command::new("test").about("Some help"))
        .subcommand(
            Command::new("show")
                .about("Show costume")
                .help_heading("Costume Commands"),
        );

    utils::assert_output(cmd, "clap-test --help", VISIBLE_ALIAS_HELP, false);
}

```

## Tests for Phase 2

```rust
fn test_with_different_custom_for_cmds_and_help() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Custom Commands:
  test  Some help

Custom Help:
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Custom Help")
        .next_help_heading("Custom Commands")
        .subcommand(Command::new("test").about("Some help"));

    utils::assert_output(cmd, "clap-test --help", VISIBLE_ALIAS_HELP, false);
}

fn test_two_custom_headings_for_cmd_and_custom_heading_on_help() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Custom Commands:
  test  Some help

Costume Commands:
  show  Show costume

Custom Help:
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Custom Help")
        .next_help_heading("Custom Commands")
        .subcommand(Command::new("test").about("Some help"))
        .subcommand(
            Command::new("show")
                .about("Show costume")
                .help_heading("Costume Commands"),
        );

    utils::assert_output(cmd, "clap-test --help", VISIBLE_ALIAS_HELP, false);
}

```

---

_Comment by @epage on 2025-11-10 21:15_

> > > Command::custom_heading_for_help

> > In theory, this is short-lived. 

> I don't see it quite that way.

See https://github.com/clap-rs/clap/issues/1553#issuecomment-3492559026

> So to override the help heading for `--help` and `--version`, you need to disable the built-in versions and provide your own.
> 
> So for parity, the ideal would be if you could define your own `help` command and get the same behavior. At minimum, this would require we add `CommandAction`, like `ArgAction`. Note that this would be an action that runs when a command is first encountered and not pluggable actions after a command has run which has been requested from time to time (e.g. [clap-nested](https://crates.io/crates/clap-nested)). This is a larger effort with many unknowns to be worked out.



---

_Comment by @jerusdp on 2025-11-13 07:49_

I see, if I understand it correctly then the assumption is that `Command::custom_heading_for_help` is short lived being deprecated when `CommandAction` is implemented to allow custom headings to be placed on the a `help` command in the same way as it is placed on the flags in the options today by implementing a custom help command.  This leaves me wondering what the prime driver for the arg solution was and whether solving the help heading is a side effect of a having a action to allow the substitution/renaming of the flags used for help and version. The test examples and document demonstrate the use of `-?` instead of `-h` as the help short flag and it seems reasonable as this is a common short cut, particularly on windows programs. I wonder if the same driver is there for the replacement of the help command?

With the proposed solution replacement of header for both command arg is tested as follows:

```rust
fn test_custom_headings_help_and_help_options() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Commands:
  test  Some help

Auto tools:
  help  Print this message or the help of the given subcommand(s)

Auto tools:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Auto tools")
        .disable_help_flag(true)
        .disable_version_flag(true)
        .arg(
            Arg::new("custom-help")
                .short('h')
                .long("help")
                .help_heading("Auto tools")
                .help("Print help")
                .action(clap::ArgAction::Help),
        )
        .arg(
            Arg::new("custom-version")
                .short('V')
                .long("version")
                .help_heading("Auto tools")
                .help("Print version")
                .action(clap::ArgAction::Version),
        )
        .subcommand(Command::new("test").about("Some help"));

    utils::assert_output(cmd, "clap-test -h", VISIBLE_ALIAS_HELP, false);
}
```

A consistency argument could be made to apply the same header as configured for the command to the arguments to avoid replacing the standard flags with custom "look-a-likes". 

This might look as follows and perhaps the "Command::custom_heading_for_help()` API needs a better name to reflect that the change is applied to all auto-generated commands and options. 

```rust
fn test_custom_headings_help_and_help_options() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Commands:
  test  Some help

Auto tools:
  help  Print this message or the help of the given subcommand(s)

Auto tools:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .custom_heading_for_help("Auto tools")
        .subcommand(Command::new("test").about("Some help"));

    utils::assert_output(cmd, "clap-test -h", VISIBLE_ALIAS_HELP, false);
}
```



---

_Comment by @epage on 2025-11-13 21:18_

As I said, we've intentionally been moving *away* from functions that offer one-off customization of built-ins and instead been focusing on having people provide their own variant of it.

---

_Comment by @jerusdp on 2025-11-13 22:18_

In that case and if the preferred solution for the `help` command is that the user provides a customised function requiring `CommandAction` which is not in scope for this change the initial proposal seems the most sensible rather than implementing something that will be short-lived: if you want to change the section under which the `help` is listed then you amend the `Command` heading. 

---

_Label `S-waiting-on-design` removed by @epage on 2025-11-13 22:41_

---

_Label `E-easy` added by @epage on 2025-11-13 22:41_

---

_Comment by @epage on 2025-11-13 22:44_

I'm fine with moving this first phase forward with using `Command::subcommand_help_heading` as a stop gap until `CommandAction`s are available.

---

_Comment by @jerusdp on 2025-11-14 08:35_

I think we are making some progress.


## Introduction 

The overall objective is to provide the user with a facility to place subcommands under more specific headings. 

### Vision for final solution

The final solution should align the behaviour for commands with that already configured for arguments, meaning that:

1. that headings can be specified when a command is configured
2. that Command::next_help_heading can be used as a default heading for commands
3. that  a custom  `help` command can be configured (providing a custom heading if required)

### Scope of this PR

The objective for this PR is to provide a facility for the user to place their commands under a custom header to allow the grouping of commands such as might be seen in `git --help`. 

The PR will not implement `CommandAction` which would be required to permit the user to replace the `help` command with a custom user `help` command which could be place under a custom header. The user can place `help` under a custom header by amending the default title using `Command::subcommand_help_heading` placing the help command and the updated section at the top of the list of commands. 

The will not change the scope of the  `Command::next_help_heading` to include processing for commands as this is a breaking change. 

## Design Statements

| Statement | Comment |
| --- | --- |
| MUST provide API to specify a custom section header for a user command | The key objective of the change |
| MUST keep commands with the same header together | |
| MUST display commands with no custom header under the default header | |
| MUST display help command under the standard header | To amend this the user should change the default header |
| WILL NOT change scope of `Command::next_help_heading` to include subcommands | | 
| WILL NOT deprecate `Command::subcommand_help_heading` | It can be used to generate custom section heading for help |
| WILL NOT implement custom replacement of help | Future solution requiring `CommandAction` 

## High Level Design

### Custom help headings

#### Addressing:

- MUST provide API to specify a custom section header for a user command
- MUST keep commands with the same header together
- MUST display commands with no custom header under the default header
- MUST display help command under the standard header

Provide API Command::help_heading to set the current help heading and associated field (similar to Arg::help_heading). Document workaround for user to configure a custom heading on help command. Provide tests and examples that demonstrate the configurations. 

## Design Vision

This design allow the following comprehensive test to pass:

```rust
fn test_multiple_commands_mixed_standard_and_custom_headers() {
    static VISIBLE_ALIAS_HELP: &str = "\
Usage: clap-test [COMMAND]

Help Section:
  help      Print this message or the help of the given subcommand(s)

Commands:
  def_cmd1  First command under default command heading
  def_cmd2  Second command under default command heading
  def_cmd3  Third command under default command heading
  def_cmd4  Fourth command under default command heading

First Custom:
  ch1_cmd1  First command under first custom command heading
  ch1_cmd2  Second command under first custom command heading
  ch1_cmd3  Third command under first custom command heading
  ch1_cmd4  Fourth command under first custom command heading

Second Custom:
  ch2_cmd1  First command under second custom command heading
  ch2_cmd2  Second command under second custom command heading
  ch2_cmd3  Third command under second custom command heading
  ch2_cmd4  Fourth command under second custom command heading

Options:
  -h, --help     Print help
  -V, --version  Print version
";

    let cmd = Command::new("clap-test")
        .version("2.6")
        .subcommand_help_heading("Help Section")
        .subcommand(
            Command::new("def_cmd1")
                .about("First command under default command heading")
                .help_heading("Commands"),
        )
        .subcommand(
            Command::new("def_cmd2")
                .about("Second command under default command heading")
                .help_heading("Commands"),
        )
        .subcommand(
            Command::new("def_cmd3")
                .about("Third command under default command heading")
                .help_heading("Commands"),
        )
        .subcommand(
            Command::new("def_cmd4")
                .about("Fourth command under default command heading")
                .help_heading("Commands"),
        )
        .subcommand(
            Command::new("ch1_cmd1")
                .about("First command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch1_cmd2")
                .about("Second command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch1_cmd3")
                .about("Third command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch1_cmd4")
                .about("Fourth command under first custom command heading")
                .help_heading("First Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd1")
                .about("First command under second custom command heading")
                .help_heading("Second Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd2")
                .about("Second command under second custom command heading")
                .help_heading("Second Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd3")
                .about("Third command under second custom command heading")
                .help_heading("Second Custom"),
        )
        .subcommand(
            Command::new("ch2_cmd4")
                .about("Fourth command under second custom command heading")
                .help_heading("Second Custom"),
        );

    utils::assert_output(cmd, "clap-test --help", VISIBLE_ALIAS_HELP, false);
}
```

---

_Comment by @epage on 2025-11-14 21:51_

Unless I missed a discrepancy, that looks like the first step that I said we were good to go forward with.

---

_Referenced in [clap-rs/clap#6183](../../clap-rs/clap/pulls/6183.md) on 2025-11-17 12:11_

---

_Referenced in [clap-rs/clap#6185](../../clap-rs/clap/issues/6185.md) on 2025-11-18 16:27_

---

---
number: 1350
title: How to manually print help for subcommands?
type: issue
state: closed
author: ehuss
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-author
assignees: []
created_at: 2018-09-28T16:24:42Z
updated_at: 2022-06-02T14:31:13Z
url: https://github.com/clap-rs/clap/issues/1350
synced_at: 2026-01-07T13:12:19-06:00
---

# How to manually print help for subcommands?

---

_Issue opened by @ehuss on 2018-09-28 16:24_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Affected Version of clap

clap 2.32.0

### Bug or Feature Request Summary

Is there a way to manually print the help output for subcommands?

Calling `print_help` on the base App only prints help for the base app.  Calling it on a subcommand App object means the help will be incomplete (doesn't include base app options, the command name is wrong, etc.).

To give more context, I am experimenting with having the `--help` option print the short help, and `--help --verbose` print long help.  I was going down the route of manually adding a "help" arg (and subcommand), but was unable to figure out a way to print the help for subcommands.

---

_Referenced in [rust-lang/cargo#6104](../../rust-lang/cargo/issues/6104.md) on 2018-10-04 00:44_

---

_Comment by @xoac on 2019-02-19 12:58_

I have similar problem. I use `app read` sub-command and `read` is sub-command that do nothing it's just should print help that you can use `app read sub1` or `app read sub2` etc. 

---

_Comment by @ehuss on 2019-02-19 14:46_

@xoac I think you can use [SubcommandRequiredElseHelp](https://docs.rs/clap/2.32.0/clap/enum.AppSettings.html#variant.SubcommandRequiredElseHelp) on your `read` subcommand to accomplish that.

---

_Comment by @sharkdp on 2019-08-01 18:52_

I also have a use case for this: I have a subcommand (`subcmd`) that can either read its arguments from the command line via positional arguments (`app subcmd arg1 arg2 arg3`) or read the arguments from a file, if no positional arguments are given: (`other_command | app subcmd`).

If `app subcmd` is called without any arguments and `STDIN` is attached to a TTY (i.e. nothing is piped into the command), I want to show the help message of `subcmd`, because it is unlikely that the user wants to input the arguments via the keyboard.

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-02 04:41_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-02 04:41_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-02 04:41_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-02 04:41_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 04:41_

---

_Referenced in [mike-engel/jwt-cli#50](../../mike-engel/jwt-cli/issues/50.md) on 2020-02-02 12:07_

---

_Comment by @monoclex on 2020-10-10 19:38_

I also have a usecase - I'd like to create an app which, upon running a command with `--help` (such as `myprogram do thing --help`) would provide information about what would happen when it "does thing" (sort of like a --dry-run argument)

---

_Comment by @pksunkara on 2020-10-10 19:41_

@SirJosh3917 I don't think your use case fits here. And it is actually achievable with the current master by using the app setting `DisableHelpFlags`.

---

_Comment by @monoclex on 2020-10-10 19:54_

@pksunkara In a scenario where my app can't provide any information (i.e. the user runs `myprogram --help`), I'd like to print to the user the help that would clap would normally print, which is where this issue would solve that - by allowing me to print the current help. Sorry if my use-case was unclear, and thank you for pointing me towards the desired app setting!

---

_Comment by @pksunkara on 2020-10-10 20:16_

I still don't understand it. Can you please create a discussion with mock details and expected behavior?

---

_Comment by @monoclex on 2020-10-11 00:27_

Here's a mockup of some pseudo program:

```
> program install package
Installing "package" v 1.0.0...
Complete!

> program install another-package --help
(i) help requested | program install another-package
- installation location: `./here` from environment variable INSTALL_LOCATION
- found "another-package" at version "1.2.3"

> program install --help
(i) help requested | program install
- installation location: `./here` from environment variable INSTALL_LOCATION
- required argument "<package>" missing. printing command usage:
program-install
install a dependency with program

USAGE:
    program install <package>

FLAGS:
    -h, --help        Gets help

ARGS:
    <package> - the package

> program install
error: required argument <package> not specified
program-install
install a dependency with program

USAGE:
    program install <package>

FLAGS:
    -h, --help        Gets help

ARGS:
    <package> - the package

> program
(i) downloading all dependencies...
done!

> program
/!\ no dependency.list exists, make sure one exists if you're trying to download dependencies! displaying help:
program
does things

USAGE:
    program [subcommand] ...

ARGS:
    -h, --help      does help

SUBCOMMANDS:
    install         installs
```

As you can see, the situation for when help is displayed varies wildly. Ideally, given an `&App` or `&SubCommand`, to turn that into a `&str` that can then be used by users of the clap library to do what they wish with. For my specific usecase, it would be nice if there was an easy way to check if `--help` was supplied for a given `ArgMatches`.

---

_Comment by @pksunkara on 2020-10-11 06:53_

@SirJosh3917 Here you can just print the help of the subcommand using `print_help` unlike what the original request is asking about.

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#104](../../epage/clapng/issues/104.md) on 2021-12-06 17:37_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:12_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 18:42_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 18:42_

---

_Label `S-waiting-on-design` removed by @epage on 2021-12-09 18:42_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 18:42_

---

_Label `S-waiting-on-mentor` removed by @epage on 2022-01-07 13:44_

---

_Label `S-waiting-on-design` added by @epage on 2022-01-07 13:44_

---

_Comment by @epage on 2022-01-07 13:49_

I think the main question I have on this is what the API would look like.  Printing the help for a subcommand usually requires context from the parent `App`.  So it seems like the API would need to be `App::print_help(&mut self, subcommands: impl Iterator<Item=&str>) -> std::io::Result<()>`.  **This would require manually building up the list of nested subcommands.  Does that work?**

If we were willing to keep `App` around during `ArgMatches` (so far that has intentionally been avoided to allow apps to keep total memory usage down assuming apps keep their `ArgMatches` around through their lifetime) we could attach to it a proxy object that could handle it for us, so you could just ask the appropriate `ArgMatches` for your subcommand and it would give you what you want.

Whatever our approach, we should keep in mind help, usage, and error `App` functions.

---

_Comment by @epage on 2022-05-04 21:45_

The following tests prints the help of a subcommand
```rust
#[test]
fn multicall_render_help() {
    static EXPECTED: &str = "\
foo-bar 1.0.0
USAGE:
    foo bar [value]
ARGS:
    <value>    
OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information
";
    let mut cmd = Command::new("repl")
        .version("1.0.0")
        .propagate_version(true)
        .multicall(true)
        .subcommand(Command::new("foo").subcommand(Command::new("bar").arg(Arg::new("value"))));
    cmd.build();
    let subcmd = cmd.find_subcommand_mut("foo").unwrap();
    let subcmd = subcmd.find_subcommand_mut("bar").unwrap();

    let mut buf = Vec::new();
    subcmd.write_help(&mut buf).unwrap();
    utils::assert_eq(EXPECTED, String::from_utf8(buf).unwrap());
}
```

Key elenents
- Call `cmd.build()`
- Recurse into the intended subcommand with `find_subcommand_mut`
- Call `write_help` or `print_help` on the discovered subcommand

Is that sufficient for this?

---

_Label `S-waiting-on-design` removed by @epage on 2022-05-04 21:45_

---

_Label `S-waiting-on-author` added by @epage on 2022-05-04 21:45_

---

_Comment by @epage on 2022-06-02 14:31_

Without further feedback, I'm going to assume the given answer is sufficient for this.

---

_Closed by @epage on 2022-06-02 14:31_

---

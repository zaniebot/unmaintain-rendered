---
number: 1361
title: Support for pacman-style multi-level arguments
type: issue
state: closed
author: ncihnegn
labels: []
assignees: []
created_at: 2018-10-15T06:34:17Z
updated_at: 2020-07-18T14:28:57Z
url: https://github.com/clap-rs/clap/issues/1361
synced_at: 2026-01-10T01:26:50Z
---

# Support for pacman-style multi-level arguments

---

_Issue opened by @ncihnegn on 2018-10-15 06:34_

### Bug or Feature Request Summary
I want to emulate archlinux pacman's style with two-level arguments.
I don't think clap can support this yet.
It feels like subcommands but it is still arguments.


### Expected Behavior Summary
app -h shows the first level arguments.
app -Qh shows the second level arguments for first level argument Q.

Example:
First level: pacman -h

> usage:  pacman <operation> [...]
> operations:
>     pacman {-h --help}
>     pacman {-V --version}
>     pacman {-Q --query}    [options] [package(s)]
>     pacman {-R --remove}   [options] <package(s)>
>     pacman {-S --sync}     [options] [package(s)]
>     pacman {-U --upgrade}  [options] <file(s)>

Second level: pacman -Sh

> usage:  pacman {-S --sync} [options] [package(s)]
> options:
>    -y, --refresh        download fresh package databases from the server

### Actual Behavior Summary

> FLAGS:
>     -h, --help       Prints help information
>     -Q, --query
>     -y, --refresh
>     -R, --remove
>     -S, --sync
>     -U, --upgrade
>     -V, --version    Prints version information




---

_Referenced in [clap-rs/clap#1404](../../clap-rs/clap/issues/1404.md) on 2019-02-03 20:36_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 15:50_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-01 15:50_

---

_Comment by @NickHackman on 2020-04-13 23:16_

I think this would be a cool feature to implement, if you don't mind I'd like to clarify my approach and see if that's a good one. Prior to proceeding forward. :slightly_smiling_face: 

First problem clarification,
The goal is to be able to have sub-commands of both `long` and `short` that are in the form of Flags and allow them to be combined with flags?

## Potential Syntax
```rust
App::new("MyApp")
	.version(crate_version!())
	.author(crate_author!())
	.subcommand(
		App::new_flag("S", "sync")
			.arg(
				Arg::with_name("refresh")
				.short("y")
				.long("refresh")).get_matches();

// Goal is to allow
// MyApp -Sy and MyApp -yS
```

Only downside I see is that it could allow for the top level `App::new` call to be a `App::new_flag` instead.

## Approach

Some modification of `App` would be required, specifically a way to differentiate between the standard `new` and `new_flag` (probably an `Enum` with two variants that hold the passed in values), which then can be used in the actual parsing to count it as a `flag` instead of as a sub-command.

```rust
enum AppType {
	Normal(String),
	Flag(String, String),
}
```

Tell me what you guys think, if this is something that you'd want in the project at all, if my approach sounds reasonable. Feedback would be greatly appreciated :+1: 

---

_Comment by @CreepySkeleton on 2020-04-14 00:54_

@NickHackman Nice to see you're interested! 👌  Feel free to ask for anything you need.

Regarding syntax: we must support both long and short option subcommands, like `-Q` and `--query` in the first comment. I have some ideas but I'd like to hear your fresh opinion first 😉 

Implementation... well, it will most certainly require modifications in `App`, and it will also require modifications in the parsing process: currently almost everything that starts with `-` will be treated as option. I believe we can give you a hand on that.

---

_Comment by @NickHackman on 2020-04-14 05:07_

> currently almost everything that starts with - will be treated as option. I believe we can give you a hand on that.

Exactly, so I think the easiest solution would be...

## Solution
1. use enum mentioned above
2. add `App::new_flag`
3. In `parse_short_arg` and `parse_long_arg` add a check for any of the `flag_subcommands` then call `parse_subcommand` instead of `parse_flag`
4. Slight modification of `parse_subcommand` to play nice with the `Enum`

## Performance

- In `parse_short_arg` this will not cause an issue because it fails afterwards.
- Could be slightly hurt in `parse_long_arg`, but by a minor amount.

@CreepySkeleton Is that what you were thinking as well? Any advice is appreciated :slightly_smiling_face: 

If we're on the same page, I'll start on the implementation and open a PR in the coming days!

---

_Comment by @CreepySkeleton on 2020-04-17 01:29_

@NickHackman Sorry for the delay, so many things to do 😭 

The problem is: how to pass the long and short names in?

Options:

* Add `long` and `short` methods to `App` that are usable only in this contaxt (and panic otherwise). Dirty.
* Pass them right to constructor: `App::flag_subcommand("sub", Some("-S"), Some("--sync"))`. Not so handy, and have to panic if both are `None`.
* Another builder?

Regarding implementation, more or less correct. I'm not really sure myself 😄 

---

_Comment by @NickHackman on 2020-04-17 05:09_

@CreepySkeleton completely on the same page, so I want to preserve the current interface and its consistency as much as possible, but it feels like all the options are fairly hacky.

> Add long and short methods to App that are usable only in this context (and panic otherwise). Dirty.

Completely agree, this feels dirty.

> Pass them right to constructor: App::flag_subcommand("sub", Some("-S"), Some("--sync")). Not so handy, and have to panic if both are None.

I had a similar idea `App::new_flag<Into<String>>(short: char, long: S)`, I was thinking of using the `long` as the `App.name`, overall doesn't feel good. Mostly because `App` is also the highest level (read as not a subcommand), a constructor for essentially a subcommand doesn't feel right. Passing in `Options` always feels a little dirty. This doesn't seem like a bad idea, but feels inconsistent in regards to the rest of Clap.

> Another builder?

I don't like this idea because it's a breaking change, but it feels the most consistent to create a `Subcommand` rather than using `App` and furthermore a `FlagSubcommand`, but this could be a lot of duplicated code, and requires a lot of changes all over.

I'm still down to work on this issue, but figuring out a good way for it to mesh with the overall interface is most of the battle here.

I really appreciate your help and feedback, thanks 😄

---

_Comment by @pksunkara on 2020-04-17 06:34_

This is actually super easy with the replace field added in #1697. The only design issue is that you would have define the subcommand (which we already do) and then define the flags for it again (instead of defining them with subcommand).

---

_Comment by @NickHackman on 2020-04-18 08:32_

```rust
use clap::{App, Arg};

fn main() {
    let matches = App::new("pacman")
        .subcommand(
            App::new("sync").args(&[
                Arg::with_name("search")
                    .short('s')
                    .long("search")
                    .help("search remote repositories for matching strings"),
                Arg::with_name("info")
                    .short('i')
                    .long("info")
                    .help("view package information (-ii for extended information)"),
                Arg::with_name("refresh").short('y').long("refresh").help(
                    "download fresh package databases from the server (-yy to force a refresh even if up to date)",
                ),
            ]),
        )
        .replace("-S", &["sync"])
        .replace("--sync", &["sync"])
        .get_matches();

    println!("{}", matches.is_present("sync"));
}

```

Is a partial test case using `replace`, sadly I don't believe it handles the cases of `pacman -Sy` or `pacman -yS`, please correct me if I'm improperly using it in any way. Do you have any suggestions on how to handle this to keep the current Clap interface consistent? 🙂


---

_Comment by @pksunkara on 2020-04-18 09:25_

Try using this and extending it. Long options are working now, so, see if you can use this replace while parsing the short options and replace stuff there. If not, try something in a similar way.

Also, `pacman -yS` means `pacman -y sync` or `pacman sync -y`?

---

_Comment by @CreepySkeleton on 2020-04-18 11:20_

@pksunkara `pacman -yS` means `-y -S` (two simple flags) since `-y` is not a subcommand. Most likely, this is an error. Replace won't do here due to `-Sy` cases (subcommand chained with an option).

@NickHackman 

Regarding API: the number of possible variants is 3, so we can make something akin `SubCommand` in clap 2.x used to be:
```rust
struct FlagSubCommand {}

impl FlagSubCommand {
    fn new_short(id: &str, short: char) -> App;

    fn new_long(id: &str, long: &str) -> App;

    fn new_short_long(id: &str, short: char, long: &str) -> App;
}
```

Note: they all return `App`.

We can also go this way

> Add long and short methods to App that are usable only in this context (and panic otherwise). 

Dirty, but easy to use and implement.

>  figuring out a good way for it to mesh with the overall interface is most of the battle here.

let me tell you a secret: like, 80 of discussions and disputes here are about "how to mend in with existing interface" entirely 🤣 

---

_Comment by @NickHackman on 2020-04-19 16:39_

> pacman -yS means -y -S (two simple flags) since -y is not a subcommand. Most likely, this is an error. Replace won't do here due to -Sy cases (subcommand chained with an option).

Actually, it propagates the `-y` down to the subcommand, this behavior definitely sounds more permissive and less performant. Clap doesn't provide this behavior (as far as I know) nor should it, it's a weird case for Command line argument parsing.

> Regarding API: the number of possible variants is 3, so we can make something akin SubCommand in clap 2.x used to be:

I like this idea. I'll work on this and have it done ~by at least next weekend~. (Setting a deadline for myself that's fairly lenient)

> let me tell you a secret: like, 80 of discussions and disputes here are about "how to mend in with existing interface" entirely :rofl: 

Yeah, integrating with and keeping an interface together is hard :joy:

**EDIT**: Sorry, a lot going on with finals and all by next weekend.


---

_Comment by @rami3l on 2020-06-10 14:54_

Just being curious, any news on this topic?

I'm using `clap` to write [`pacaptr`](https://github.com/rami3l/pacaptr) (it's a Pacman syntax wrapper for other package managers), and this feature must be a great help. 🚀

---

_Comment by @NickHackman on 2020-06-10 16:25_

Sorry, got a little sidetracked. I'll start working on this now :smile: 

---

_Referenced in [clap-rs/clap#1974](../../clap-rs/clap/pulls/1974.md) on 2020-06-13 22:01_

---

_Closed by @bors[bot] on 2020-07-18 14:28_

---

_Referenced in [epage/clapng#111](../../epage/clapng/issues/111.md) on 2021-12-06 17:39_

---

_Referenced in [clap-rs/clap#3470](../../clap-rs/clap/pulls/3470.md) on 2022-02-14 22:26_

---

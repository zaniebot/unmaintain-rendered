---
number: 2222
title: Support command chaining
type: issue
state: open
author: fkrauthan
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2020-11-24T18:04:55Z
updated_at: 2025-07-09T14:11:25Z
url: https://github.com/clap-rs/clap/issues/2222
synced_at: 2026-01-07T13:12:19-06:00
---

# Support command chaining

---

_Issue opened by @fkrauthan on 2020-11-24 18:04_

### Describe your use case

I would like to re-write an existing internal Python tool in rust. For this python tool we use click with the command chaining feature (https://click.palletsprojects.com/en/7.x/commands/#multi-command-chaining) this allows similar to some bigger CLI tools (e.g. `gradle` or `setup.py`) to execute multiple commands in one go. In our example we have a simple build and deploy tool and people often call it ether via `./tool build start` or via `./tool start logs` etc to trigger different functionality in one go. Where `build` would build our system; `start` would startup the code locally and `logs` would tail the logs.

### Describe the solution you'd like

A simple switch that allows me to enable command chaining. I am ok with that prevents me from using sub commands and instead I can only use one level of commands. Examples of implementations are:

* [Click](https://click.palletsprojects.com/en/7.x/commands/#multi-command-chaining) in Python
* [clikt](https://ajalt.github.io/clikt/commands/#chaining-and-repeating-subcommands) in Kotlin


---

_Label `T: new feature` added by @fkrauthan on 2020-11-24 18:04_

---

_Referenced in [clap-rs/clap#1434](../../clap-rs/clap/issues/1434.md) on 2021-05-26 03:17_

---

_Referenced in [epage/clapng#170](../../epage/clapng/issues/170.md) on 2021-12-06 20:19_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:12_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:12_

---

_Label `A-parsing` added by @epage on 2021-12-08 21:12_

---

_Referenced in [clap-rs/clap#1407](../../clap-rs/clap/issues/1407.md) on 2021-12-11 01:12_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 15:10_

---

_Comment by @epage on 2021-12-13 15:16_

Interesting idea!

It'd help if someone explored the nuances of how click worked so we can build on their shoulders.  What limitations does it have? How does it impact various UX features?  What could be improved?

I'm assuming any App that has this enabled would disallow sub-subcommands because that can be ambiguous with chaining.  We do this with checks in `src/build/app/debug_asserts.rs`

We'd need to decide on how to expose this in `ArgMatches`.
- Switch `ArgMarches::subcommand*` to returning iterators of active subcommands
- Despite chained subcommands existing at the same level within the `App` hierarchy, recursively nest them in `ArgMatches` as if they were arbitrary depth sub-subcommands

---

_Referenced in [rosetta-rs/argparse-rosetta-rs#50](../../rosetta-rs/argparse-rosetta-rs/pulls/50.md) on 2022-09-10 01:25_

---

_Referenced in [clap-rs/clap#4424](../../clap-rs/clap/issues/4424.md) on 2022-10-26 14:51_

---

_Comment by @0xForerunner on 2022-10-26 15:08_

Copying over an example from another thread. This is a demonstration of how this could look using derive. Ideally this should be implemented for any type that is IntoIterator<Item = T>. 

```rust
#[derive(Parser)]
#[clap(rename_all = "snake_case")]
pub enum MyCli {
    GetPrices {
        #[command(subcommand)]
        assets: Vec<AssetInfo>,
    },
}

#[derive(Subcommand)]
#[clap(rename_all = "snake_case")]
pub enum AssetInfo {
    Token {
        /// <string>, "atom3h6lk23h6"
        contract_addr: String
    },
    NativeToken {
        /// <string>, "uatom"
        denom: String
    },
}
```
This is a much simpler case then I have in reality, but useful for examining how this could work. An example run of the program could look like:
```bash
my-cli get_prices token atom3h6lk23h6 native_token uatom 
```
When these example args are parsed it would result in: 
```rust
MyCli::GetPrices{
    assets: vec![
        AssetInfo::Token { contract_addr: atom3h6lk23h6 },
        AssetInfo::NativeToken { denom: uatom }
    ]
}

---

_Comment by @0xForerunner on 2022-10-26 15:13_

> (e.g. do we stop chaining when entering a sub-sub-command or do we back out to the parent chained command?)

Why would we need to stop chaining when entering a subcommand?

---

_Comment by @epage on 2022-10-26 15:26_

Allowing both chaining and sub-subcommands has the potential for ambiguity (in practice and from the users perspective) and implementation complexity.

What will help is someone exploring what precedence there is in other implementations of chaining, exploring the various cases for how to avoid ambiguity, and exploring the use cases for why someone would need each kind of behavior we would implement for this.

---

_Comment by @0xForerunner on 2022-10-26 15:33_

It should be possible to have some check to prevent ambiguities but yes I see the problem you're referring to. As far as use case goes for me it's as simple as trying to force clap to work as well as possible with any arbitrary enum structure using its derive functionality.

---

_Comment by @abey79 on 2023-02-23 20:46_

> It'd help if someone explored the nuances of how click worked so we can build on their shoulders. What limitations does it have? How does it impact various UX features? What could be improved?

FWIW, [here is](https://click.palletsprojects.com/en/8.1.x/commands/#multi-command-chaining) the relevant paragraph in Click's documentation. The limitations are:
- no nested commands
- you can't have arguments with arbitrary number of value (except for the last command)
- the order `--options argument` is enforced (e.g. you can't do `mycli cmd1 arg1 --opt1 val   cmd2 [...]`


---

_Comment by @brownjohnf on 2023-03-27 18:07_

I found this issue via #4424 and wanted to add my use-case. I'm not sure how this would work implementation-wise, but it'd be very useful:

I'm building a fairly complex CLI that supports a dynamic number of duplicate configs. It's proxying traffic via different protocols, and adding filters on top of them. So for example, I might want to listen on tcp and udp, and parse tcp messages as strings but the udp messages as protobufs, and log the raw bytes first (just random simple examples). I can think of two logical approaches, and would be content if either were supported by clap; I'd just use whichever it supported.

The first is `find`-style layered/ordered flags:

`$ mycli --listen tcp://localhost:1234 --filter str --filter log --listen udp://localhost:2345 --filter log --filter protobuf`

The second is more what this issue (and #4424) is discussing, if I understand correctly:

`$ mycli listen --uri tcp://localhost:1234 --filter str --filter log listen --uri udp://localhost:2345 --filter log --filter protobuf`

I personally find the first pattern more intuitive because of prior exposure to things like the `find` command, but the second is probably more intuitive, and has the advantage of supporting more robust built-in argument validation/parsing using clap derive attributes (would be easier to express things like combinatoric flag constraints per-subcommand). An example of an in-the-wild use of the ordered find arguments, which is running gofmt on all `*.go` files in the current directory recursively, but pruning out any files in `./vendor`:

`gofmt -l $(find . -path ./vendor -prune -o -name '*.go' -print)`

I've considered solving this short-term by requiring that you pass a number of --filter arguments equal to --uri arguments, but this starts breaking down with variable number of filters, or other arguments that should only apply to one of the listeners. It also requires doing a lot of custom parsing if you want to group multiple arguments (e.g. make --filter a `Vec<Vec<String>>` and accept multiple --filter flags, each with comma-delimited strings that get parsed into the nested Vec).

If someone knows a way to achieve this already in clap, I'd love to hear it, and otherwise would love to see support get added. Clap has been such a fabulous and pleasant asset when working with rust CLIs.

---

_Comment by @epage on 2023-03-27 18:33_

@brownjohnf thanks for sharing!

While its not the most ergonomic and doesn't have built-in support for the derive API, we have an [example of a `find`-like application](https://docs.rs/clap/latest/clap/_derive/_cookbook/find/index.html)

---

_Comment by @brownjohnf on 2023-03-27 18:36_

Ah, amazing! I'd missed that, and on first glance it looks like it'll probably solve my problem in the short-term. Thanks so much.

---

_Comment by @abey79 on 2023-03-27 20:28_

@brownjohnf FYI, in [this crate](https://github.com/abey79/vsvg/tree/master/vsvg), I've pushed the "find" example to some extremities. I support repeated options and order, which the "find" example doesn't support IIRC. I also made some macros to reduce the boilerplate of defining commands. There are loads of limitations to it and, being new to Rust, this is likely messy and non-idiomatic on multiple counts, but it gets me far enough for this project.

---

_Comment by @brownjohnf on 2023-03-28 22:18_

Wow, there's a lot happening there :-) I haven't digested all of it, but 
at some point it seems like doing it that way is more trouble than just 
manually parsing args from argv with something like nom (at least for my 
use case). I'd _love_ to see repeated subcommands get added to clap. 
It'd solve all my problems... I'd be happy to help contribute it, if we 
came up with a design. I don't really know anything about clap 
internals, and have only ever used it via the derive macros.

On 3/27/23 13:28, Antoine Beyeler wrote:
> @brownjohnf FYI, in [this crate](https://github.com/abey79/vsvg/tree/master/vsvg), I've pushed the "find" example to some extremities. I support repeated options and order, which the "find" example doesn't support IIRC. I also made some macros to reduce the boilerplate of defining commands. There are loads of limitations to it and, being new to Rust, this is likely messy and non-idiomatic on multiple counts, but it gets me far enough for this project.
> 


---

_Comment by @epage on 2023-03-29 00:08_

For this to move forward, we need
- A summary of the expected behavior
  - Ideally also with a comparison to prior art
- A proposal that includes
  - How to enable it
  - How it should show up in help
  - How to update [`ArgMatches`] to access the chained commands
  - How to expose this in `clap_derive`

---

_Comment by @abey79 on 2023-03-29 09:04_

OK, here is my take.

Coming from Click, [this paragraph](https://click.palletsprojects.com/en/8.1.x/commands/#multi-command-chaining) describes pretty well what I would expect. For background, I created and maintain a Click-based CLI pipeline called [*vpype*](https://github.com/abey79/vpype). I think it's an excellent example of how a chained multi-command pipeline CLI should behave (but I'm slightly biased).

Note that I make this proposal with a very minimal knowledge of Clap's internals and a rather thin experience of actually using the library. So it may very well make little sense. In exchange, you get a fresh/candid feedback on one person's expectations for a CLI framework "with all of the bells and whistles". ;)

Here is how I'd transform the builder tutorial to use command chaining (I marked my changes with `//NEW`):

```rust
use std::path::PathBuf;

use clap::{arg, command, value_parser, ArgAction, Command};

fn main() {
    let matches = command!() // requires `cargo` feature
        .chain(true) //NEW: enable chaining of sub commands
        .arg(arg!([name] "Optional name to operate on"))
        .arg(
            arg!(
                -c --config <FILE> "Sets a custom config file"
            )
                // We don't have syntax yet for optional options, so manually calling `required`
                .required(false)
                .value_parser(value_parser!(PathBuf)),
        )
        .arg(arg!(
            -d --debug ... "Turn debugging information on"
        ))
        .subcommand(
            Command::new("test")
                .about("does testing things")
                .arg(arg!(-l --list "lists test values").action(ArgAction::SetTrue)),
        )
        .subcommand( //NEW: let's add another command for example purposes
            Command::new("config")
                .about("configures next test(s)")
                .arg(arg!(-s --set <PARAM> <VALUE> "sets a config value")),
        )
        .get_matches();

    // You can check the value provided by positional arguments, or option arguments
    if let Some(name) = matches.get_one::<String>("name") {
        println!("Value for name: {}", name);
    }

    if let Some(config_path) = matches.get_one::<PathBuf>("config") {
        println!("Value for config: {}", config_path.display());
    }

    // You can see how many times a particular flag or argument occurred
    // Note, only flags can have multiple occurrences
    match matches
        .get_one::<u8>("debug")
        .expect("Count's are defaulted")
    {
        0 => println!("Debug mode is off"),
        1 => println!("Debug mode is kind of on"),
        2 => println!("Debug mode is on"),
        _ => println!("Don't be crazy"),
    }

    //NEW: iterate over sub-commands
    for subcmd in matches.subcommand_iter() {
        match subcmd.name() {
            "test" => {
                if subcmd.get_flag("list") {
                    println!("Printing testing lists...");
                } else {
                    println!("Not printing testing lists...");
                }
            }
            "config" => {
                println!("Configuring next test(s)...");
            }
            _ => unreachable!(),
        }
    }

    // Continued program logic goes here...
}
```

This would yield the following help string (directly inspired from  Click):

```
$ 01_quick --help
A simple to use, efficient, and full-featured Command Line Argument Parser

Usage: 01_quick[EXE] [OPTIONS] [name] COMMAND1 [ARGS]... [COMMAND2 [ARGS]...]...

Commands:
  test          does testing things
  config        configures next test(s)
  help          Print this message or the help of the given subcommand(s)

Arguments:
  [name]  Optional name to operate on

Options:
  -c, --config <FILE>  Sets a custom config file
  -d, --debug...       Turn debugging information on
  -h, --help           Print help
  -V, --version        Print version
```

And here is how I'd adapt the derive tutorial:

```rust
use std::path::PathBuf;

use clap::{Parser, Subcommand};

#[derive(Parser)]
#[command(author, version, about, long_about = None, chain = true)] //NEW
struct Cli {
    /// Optional name to operate on
    name: Option<String>,

    /// Sets a custom config file
    #[arg(short, long, value_name = "FILE")]
    config: Option<PathBuf>,

    /// Turn debugging information on
    #[arg(short, long, action = clap::ArgAction::Count)]
    debug: u8,

    #[command(subcommand)]
    command: Option<Commands>,
}

#[derive(Subcommand)]
enum Commands {
    /// does testing things
    Test {
        /// lists test values
        #[arg(short, long)]
        list: bool,
    },

    /// configures next test(s)
    Config { //NEW
        /// sets a config value
        #[arg(short, long)]
        set: Option<(String, String)>,
    },
}

fn main() {
    let cli = Cli::parse();

    // You can check the value provided by positional arguments, or option arguments
    if let Some(name) = cli.name.as_deref() {
        println!("Value for name: {name}");
    }

    if let Some(config_path) = cli.config.as_deref() {
        println!("Value for config: {}", config_path.display());
    }

    // You can see how many times a particular flag or argument occurred
    // Note, only flags can have multiple occurrences
    match cli.debug {
        0 => println!("Debug mode is off"),
        1 => println!("Debug mode is kind of on"),
        2 => println!("Debug mode is on"),
        _ => println!("Don't be crazy"),
    }

    //NEW: iterate over sub-commands
    for subcmd in cli.command_iter() {
        match subcmd {
            Commands::Test { list } => {
                if *list {
                    println!("Printing testing lists...");
                } else {
                    println!("Not printing testing lists...");
                }
            }
            Commands::Config { set } => {
                if let Some((param, value)) = set {
                    println!("Setting config value for {param} to {value}");
                }
            }
        }
    }

    // Continued program logic goes here...
}
```

In most cases, some dispatch mechanism (`Command` -> actual code that implements it) would be needed. This could be done statically with little boilerplate using a few macros (like I did in the project mentioned in my previous comment*), or dynamically with some code, for example if the CLI must support some kind of plug-in extensions. I'm not sure if even a minimalist implementation of either is in the scope of Clap though—it could certainly be factored in a separate crate/project.

[(*) BTW, I'm slightly surprised it was marked as off-topic, as it provides a (somewhat) working of command (well... argument) chaining, which is a work-around for the present issue. Maybe I wasn't sufficiently clear about this in that comment?]

---

_Comment by @epage on 2023-03-29 14:39_

> [(*) BTW, I'm slightly surprised it was marked as off-topic, as it provides a (somewhat) working of command (well... argument) chaining, which is a work-around for the present issue. Maybe I wasn't sufficiently clear about this in that comment?]

Oh, that whole conversation was useful but since it ran its course, I was wanting to make it easier to find the relevant parts of the conversation for command chaining and to not get too off topic.

I also feel like "argument chaining" is only a workaround in some limited cases and comment I did leave expanded will likely lead people to expand the follow ups.

---

_Comment by @epage on 2023-03-29 14:40_

> `command: Option<Commands>,`
> ...
> `for subcmd in cli.command_iter() {`


I assume that should be a `Vec`, rather than an `Option` and that it would be iterated with `&cli.commands`

---

_Comment by @abey79 on 2023-03-29 14:53_

> > `command: Option<Commands>,`
> > ...
> > `for subcmd in cli.command_iter() {`
> 
> I assume that should be a `Vec`, rather than an `Option` and that it would be iterated with `&cli.commands`

Actually, since `Commands` was defined as an enum, I extended the enum instead. It could certainly be a `Vec` of (single?) `Command` as well—I'm not sure of the implications.

On the other hand, `cli.command_iter()` would iterate on the commands that were *actually* passed on the CLI. For example, with `01_quick test config -s param 10 test`, `cli.command_iter()` would yield 3 command instances, two of which with the `Commands:Test` variant.

---

_Comment by @epage on 2023-03-29 15:05_

> Coming from Click, [this paragraph](https://click.palletsprojects.com/en/8.1.x/commands/#multi-command-chaining) describes pretty well what I would expect. For background, I created and maintain a Click-based CLI pipeline called [vpype](https://github.com/abey79/vpype). I think it's an excellent example of how a chained multi-command pipeline CLI should behave (but I'm slightly biased).

Could we be more explicit, rather than linking out to other documentation.

If nothing else, it keeps the conversation in one place so we can make sure we are satisfying all of the requirements.

For example

> When using multi command chaining you can only have one command (the last) use nargs=-1 on an argument

I assume this means that these commands effectively end the chain.
- Is there a reason to not do this for subcommands as well?
- Could we loosen this restriction since clap can handle variable length arguments and subcommands?

>It is also not possible to nest multi commands below chained multicommands.

We enforce invariants as `debug_assert`s and we need to call out in design section that we will have a debug assert for this case
- This will likely get complicated with #4792

Could we make this like the `nargs=-1` case and say that nested subcommands terminate the chain?  I haven't thought through what this does to the parser to either recurse or backtrack

---

_Comment by @epage on 2023-03-29 15:07_

> Actually, since Commands was defined as an enum, I extended the enum instead. It could certainly be a Vec of (single?) Command as well—I'm not sure of the implications.

Additional variants are just additional subcommands at the same level.  We still need a way to store (and access) multiple instances of the enum

---

_Comment by @abey79 on 2023-03-29 15:19_

> Additional variants are just additional subcommands at the same level. We still need a way to store (and access) multiple instances of the enum

If I understand you correctly (which I'm not entirely sure of), you mean that the enum in the example represent "level 1" sub-commands, which are by definition exclusive in the current (chaining-less) model, so adding variants and expecting that multiple instances exist is in contradiction with the current implementation.

I guess it's both a technical (on which I can't comment much) and API question. On the API side, I *think* I would be happier with `Vec<Command>` rather than a single `Commands` enum, because the dispatch mechanism might be easier to tuck on the `Command`s with some custom trait—but I'm pretty sure we can make do with either approaches.

What's for sure is that enforcing as single level of subcommand when `chain=true` is fair. Click has this restriction and command chaining with greater-than-1-level command hierarchies would be rather confusing from a UX point of view IMHO.

*Edit*: I'll try to answer your other message later tonight.

---

_Comment by @epage on 2023-03-29 15:47_

> If I understand you correctly (which I'm not entirely sure of), you mean that the enum in the example represent "level 1" sub-commands, which are by definition exclusive in the current (chaining-less) model, so adding variants and expecting that multiple instances exist is in contradiction with the current implementation.

Not quite. 

Take
```rust
prog test
```
and that would map to
```rust
Cli {
    name: None,
    config: None,
    debug: 0,
    command: Some(Commands::Test {
        list: false,
    })
}
```

With that said, what would the following map to?
```rust
prog test test test config
```

---

_Comment by @brownjohnf on 2023-03-29 16:30_

(sorry, re-posting from the right account :-))

From an API perspective, I'd like to see something like this:

```rust
#[derive(Parser)]
struct Cli {
    #[clap(subcommand)]
    subcmd: Vec<Subcmd>,
}

#[derive(Parser)]
enum Subcmd {
    Listen {
        #[clap(short, long)]
        port: u16,

        #[clap(short, long = "proto")]
        protocol: String, // Should be an enum, but this is an example
        // etc....
    }
}

fn main() {
    let cli = Cli::parse();

    for sub in cli.subcmd {
        println!("{:#?}", sub);
    }
}
```

And then used like this:

```shell
$ mycli listen -p 2345 listen -p 1234 # etc...
Listen {
    port: 2345,
}
Listen {
    port: 1234,
}
```

While it would be wonderful to get "arg chaining" (I guess you'd call it?) a la `find`, focusing on multiple subcommands seem like the best first step, and maybe simpler to implement. I also don't love the term chaining, because that implies they're linked to me, when in reality it should just return a Vec of subcommands that you can do whatever you want with (chain them, run them in parallel, etc.).

In terms of algorithm/parsing, as a developer I'd expect clap to:

1. Encounter a subcommand, recognize that we expect 0 or more subcommands, and begin parsing it
2. On encountering an argument that fails to parse, backtrack and re-evaluate the remaining args from the beginning.

So, if you have top-level flags or args and multi-subcommands, the subcommands would parse "greedily" until they fail, and then start all over at the beginning.

---

_Comment by @abey79 on 2023-03-30 08:54_

I've managed to confuse myself by hastily answering on the move with my previous messages—sorry for that. The `command` field would indeed need to be a vec. Also, I agree with everything that @brownjohnf just said.

> Could we be more explicit, rather than linking out to other documentation.

Right, here is (a starting point for) the spec of sub-command chaining.


#### Sub-command chaining can be enabled on a given command (defaults to disabled), and affect the way its subcommand are handled.

Builder API:
```rust
let matches = command!()
        .chain(true)
        .arg(...)
        .subcommand(...)
        .subcommand(...)
        ....
```

Derive API:

```rust
#[derive(Parser)]
#[command(chain = true, ...)]
struct Command {
    #[clap(subcommand)]
    subcmd: Vec<Subcmd>,

    ...
}
```

#### A command with chaining enabled accepts more than one sub-command.

That is kind of the point of this whole issue :)


#### A given chained sub-command may be repeated multiple times and order of invocation is maintained

This means that `cli op1 op2 op1` is allowed and is not equivalent to `cli op1 op1 op2`. Clap's API must reflect the repetition and the order.

#### Sub-command chaining may be enabled on an arbitrarily nested command

This is slightly more general than my initial take. Taking git as a well known example CLI:

```
git submodule foreach  op1 --arg  op2 -a  op3 op1
```

Here I hypothetically enabled sub-command chaining for `foreach`—itself a twice-nested subcommand—such that it accept a sequence of subcommands (`op1`, `op2`, `op3`).

This seems implied by the Click documentation, although I haven't tested that it actually works.



#### Chained sub-command may not have nested sub-command(s)

Taking the same example as before, this is not allowed:

```
git submodule foreach  op1 --arg  op2 -a subop  op3 op1
```

where `subop` would be `op2`'s sub-command.

Likewise, since `submodule` may not accept chaining sub-command as per this rule, and assuming `summary` is a sub-command of `submodule` (as is the case in the real-world git), this would not be allowed either:

```
git submodule foreach  op1 --arg  op2 -a  op3 op1  summary
```

In other word, chained sub-commands must be leaves in the command tree.

#### Chained sub-commands must parse greedily

As @brownjohnf, when it appears, a chained sub-command should eagerly parse all arguments until failure, at which point the parser must expect the next sub-command.

This implies that Click's `nargs=-1` restriction does not apply. Any chained sub-command may have any number of variable lengths arguments. (FWIW, I have actually suffered from this restriction of Click before, locally forcing me into a lesser UX.)

Technically, it think this would mean that the "chained sub-commands must be leaves" rule could be lifted as well. Both of the counter-example above could work. In the first, `subop` is recognised as `op2` sub-command so parsing continues one level down. On the second example, `summary` is not recognised as `op1`'s nor `foreach`'s sub-command, so parsing continues one level up and `summary` is recognised as sub-command of `submodule`. 

I'm not sure of the technical implication of that though, and it would certainly make for a potentially confusing UX.


<br/>

*Notes/TBD*:
- Clearly, some of these rules (in particular the "chained sub-commands must be leaves" rule) are somewhat arbitrary. I'm stating them as starting point, to trigger conversation. In particular, as @epage mention, there might be technical reasons to prefer one way or the other, and I'm fine with it.
- As @brownjohnf mentioned, I completely left aside the question of find-like argument chaining. I think starting with sub-command chaining is the correct strategy, and it covers most use-cases in a way that, I believe, is more trendy by today's standards.
- I kept using the `chain` terminology, for the lack of a better one. Although I agree with @brownjohnf that the use-case may or may not care about order, Clap should definitely keep the ordering properly, for those use-cases which need it (e.g. pipelines). `multiple` may be alternative, but it sound a bit too generic and is less explicit about the ordering aspect. I don't have another suggestion at this point.

---

_Comment by @epage on 2023-03-30 15:44_

I was looking at the parser which reminded me of some of the complexities involved, including
- Should `prog chain0 --help` list all the chained commands like `prog --help`?
- Should both levels of usage show all the chained commands?
- The parser for a chained command will need access to the parent command (for command flags, knowing whether an arg is a command or value) which will run into multiple borrows of a mutable value (`prog` owns `chain0`, `chain0` will need to be mutable for the lazy building, `prog` and `chain0` will both need to be accessible when parsing)

I'm starting to suspect that the cheapest / easiest way to implement this will be for the parent command to copy its subcommands to the command being parsed.
- It would be easy to supported subcommands under chained subcommands because we just check to see if subcommands are present and, if not, we then copy

This would cause the `ArgMatches` to be recursive and the derive would have to flatten it out.
- Maybe we could have the chained command, in the parser, flatten it out?
  - Would need to limit this to the root of the chain or else this would be a lot of moves, allocations, deallocations
  - Or we could make the caller responsible for deciding what to do with the subcommand's `ArgMatcher` and pass a `&mut Vec<ArgMatches>` down.  In the no-subcommand case, nothing should be allocated.  In the subcommand case, We just have a single-element `Vec` which we would need to switch to anyways.  In the chained-subcommand case, each layer keeps appending until we get to the root.  Most likely this will lead to an inverted list and we'd need to reverse the list.
- In general, I'm leaning towards the recursive approach.  Its the simplest, has no overhead for unchained subcommands (the more common case), and the builder API doesn't need to be as ergonomic as that is what the builder API is for
  - The main complication is the derive API knowing when to do this (it'll have to capture the state of any chaining) and knowing when to stop if we support unchained subcommands within a chained subcommand.

Except even that comes with its own challenges
- We'll also need to copy a hand-maintained list of `Command` settings that are relevant to subcommands
  - This will become more complex when we add support for plugins because then we'll need to know if a plugin should be copied into the subcommand, or even a subset of a plugin
- Subcommands under chained subcommands would get complicated if we move forward with deferred initialization of `Command` because we can't tell if subcommands are present until we build it, but we shouldn't build it until after we've copied
- This doesn't work for any call that wants to just do a `prog.build()` as that shouldn't recurse infinitely
  - Only doing it one level will work for documentation purposes (clap_mangen)
  - completions will need to do special processing in general and would likely get confused seeing the commands propagated one level
    - For dynamic completions, we could offer a `Command::build_subcommand(self, name: &str) -> Result<Command>`

---

_Comment by @abey79 on 2023-03-30 16:33_

> * Should `prog chain0 --help` list all the chained commands like `prog --help`?
> * Should both levels of usage show all the chained commands?

The way Click does it is `cli --help` shows the global options and lists the available commands, and `cli cmd --help` shows the usage of this specific command. It does not support out-of-the-box the `cli help` and `cli help cmd` semantic, and it would probably require some knowledge of the internals to emulate this behaviour.

Real world example (abbreviated):

```
$ vpype --help
Usage: vpype [OPTIONS] COMMAND1 [ARGS]... [COMMAND2 [ARGS]...]...

  Execute the sequence of commands passed in argument.

Options:
  --version           Show the version and exit.
  -h, --help          Show this message and exit.
  -v, --verbose
  -c, --config PATH   Load an additional config file.

Commands:

  Metadata:
    alpha         Set the opacity of one or more layers.
    color         Set the color for one or more layers.
    name          Set the name for one or more layers.
    pens          Apply a pen configuration.
    penwidth      Set the pen width for one or more layers.

  Primitives:
    arc           Generate lines approximating a circular arc.
    circle        Generate lines approximating a circle.
    ellipse       Generate lines approximating an ellipse.
    line          Generate a single line.
    rect          Generate a rectangle, with optional rounded angles.

  Input:
    read          Extract geometries from an SVG file.
    script        Call an external python script to generate geometries.

  Transforms:
    rotate        Rotate the geometries (clockwise positive).
    scale         Scale the geometries by a factor.
    scaleto       Scale the geometries to given dimensions.
    skew          Skew the geometries.
    translate     Translate the geometries.

$ vpype scale --help
Usage: vpype scale [OPTIONS] SCALE...

  Scale the geometries by the provided factors

Options:
  -l, --layer LID         Target layer(s).
  -o, --origin LENGTH...  Use a specific origin.
  --help                  Show this message and exit.
```

Some additional spec/remarks come to mind.

The `--help` is a short circuit argument with special handling by Click, and this behaviour is actually very useful when building a complex pipeline. This sort of scenario where the CLI building is interrupted to check the help and easily resumed by using the shell's "repeat prev command" behaviour is common in my experience:

- `vpype read input.svg layout -m 1cm -`
- What's that arg again?
- `vpype read input.svg layout -m 1cm --help`
- (*reads help of `layout` command*) Ah, yes!
- `vpype read input.svg layout -m 1cm -v middle a4 write output.svg`

For this to work, nothing must happen besides showing help when `--help` is present, regardless of the existence of other, fully-formed sub-command.

Another aspect is sub-command grouping in the top-level help, as shown above. All of these commands are "sister" sub-command of the top-level CLI—no nesting or anything. Yet they appear thematically grouped. Click doesn't support it out-of-the-box and a stack overflow recipe is needed. It's simple enough, but it would be nice to have built-in support.

Finally, in my previous message I mentioned variable length arguments. To clarify, tt would be really nice to have both variable length options *and* positional arguments. The `scale` command I used here for illustration is one such example: I'd like it have support both `scale 0.5` and `scale 0.5 1.0` for homogenous and non-homogenous 2D scaling. This is currently not possible with Click.




---

_Comment by @brownjohnf on 2023-03-30 18:22_

 > #### Chained sub-command may not have nested sub-command(s)

 > In other word, chained sub-commands must be leaves in the command tree.

I'm not sure why we'd add this restriction? It seems harder than just 
blindly following recursion as far as it goes? Once all input has been 
parsed, or no further progress can be made with remaining arguments, we 
return.

This might be a bit of a hot take, but as a developer I'm fine with 
allowing people to create confusingly unusable interfaces. I've gotten 
myself into some corners just using the existing quite rich capabilities 
in clap as it is. As an end user, I see it as either a) I'm using a 
power tool (like find) that just requires reading the manual to 
understand what it's going to do or b) the CLI author failed to design a 
reasonable interface. Neither case seems like it's clap's fault.

 > As @brownjohnf, when it appears, a chained sub-command should eagerly 
parse all arguments until failure, at which point the parser must expect 
the next sub-command.

I'd loosen this up a bit. A sub-command should eagerly parse all 
arguments until failure, at which the parser should pop up the stack and 
resume parsing the _parent_ (sub)command until failure, etc.

This allows arbitrarily nesting (sub)commands, and provided we're 
explicit about "the parser is greedy" I think it makes things easy to 
reason about, even if it does theoretically allow people to make very 
confusing CLIs. Here's an example though of something that I think makes 
sense to me (contrived, and indented for clarity):

```shell
$ mycli \
	listen -p tcp -h localhost \
	listen -p udp \
		proxy -p / --proto tcp -h 123.456.0.1 -vvv \
		proxy -p /foobar -h 145.123.2.3 --log-level ERROR \
	-v
# Interpreted as:
# With default log level of INFO, listen on tcp://localhost, and do
# nothing. listen on udp:// using default host and proxy all traffic on
# / to tcp://123.456.0.1, and enable TRACE logging. Proxy a subset of
# traffic on /foobar to upstream host 145.123.2.3 using the default
# protocol, but only log at an ERROR level.
```

That seems like a greedy recursive parser would have no issue with that 
(I realize in practice, implementing this in clap is more complex).

On 3/30/23 01:54, Antoine Beyeler wrote:
> I've managed to confuse myself by hastily answering on the move with my previous messages—sorry for that. The `command` field would indeed need to be a vec. Also, I agree with everything that @brownjohnf just said.
> 
>> Could we be more explicit, rather than linking out to other documentation.
> 
> Right, here is (a starting point for) the spec of sub-command chaining.
> 
> 
> #### Sub-command chaining can be enabled on a given command (defaults to disabled), and affect the way its subcommand are handled.
> 
> Builder API:
> ```rust
> let matches = command!()
>          .chain(true)
>          .arg(...)
>          .subcommand(...)
>          .subcommand(...)
>          ....
> ```
> 
> Derive API:
> 
> ```rust
> #[derive(Parser)]
> #[command(chain = true, ...)]
> struct Command {
>      #[clap(subcommand)]
>      subcmd: Vec<Subcmd>,
> 
>      ...
> }
> ```
> 
> #### A command with chaining enabled accepts more than one sub-command.
> 
> That is kind of the point of this whole issue :)
> 
> 
> #### A given chained sub-command may be repeated multiple times and order of invocation is maintained
> 
> This means that `cli op1 op2 op1` is allowed and is not equivalent to `cli op1 op1 op2`. Clap's API must reflect the repetition and the order.
> 
> #### Sub-command chaining may be enabled on an arbitrarily nested command
> 
> This is slightly more general than my initial take. Taking git as a well known example CLI:
> 
> ```
> git submodule foreach  op1 --arg  op2 -a  op3 op1
> ```
> 
> Here I hypothetically enabled sub-command chaining for `foreach`—itself a twice-nested subcommand—such that it accept a sequence of subcommands (`op1`, `op2`, `op3`).
> 
> This seems implied by the Click documentation, although I haven't tested that it actually works.
> 
> 
> 
> #### Chained sub-command may not have nested sub-command(s)
> 
> Taking the same example as before, this is not allowed:
> 
> ```
> git submodule foreach  op1 --arg  op2 -a subop  op3 op1
> ```
> 
> where `subop` would be `op2`'s sub-command.
> 
> Likewise, since `submodule` may not accept chaining sub-command as per this rule, and assuming `summary` is a sub-command of `submodule` (as is the case in the real-world git), this would not be allowed either:
> 
> ```
> git submodule foreach  op1 --arg  op2 -a  op3 op1  summary
> ```
> 
> In other word, chained sub-commands must be leaves in the command tree.
> 
> #### Chained sub-commands must parse greedily
> 
> As @brownjohnf, when it appears, a chained sub-command should eagerly parse all arguments until failure, at which point the parser must expect the next sub-command.
> 
> This implies that Click's `nargs=-1` restriction does not apply. Any chained sub-command may have any number of variable lengths arguments. (FWIW, I have actually suffered from this restriction of Click before, locally forcing me into a lesser UX.)
> 
> Technically, it think this would mean that the "chained sub-commands must be leaves" rule could be lifted as well. Both of the counter-example above could work. In the first, `subop` is recognised as `op2` sub-command so parsing continues one level down. On the second example, `summary` is not recognised as `op1`'s nor `foreach`'s sub-command, so parsing continues one level up and `summary` is recognised as sub-command of `submodule`.
> 
> I'm not sure of the technical implication of that though, and it would certainly make for a potentially confusing UX.
> 
> 
> <br/>
> 
> *Notes/TBD*:
> - Clearly, some of these rules (in particular the "chained sub-commands must be leaves" rule) are somewhat arbitrary. I'm stating them as starting point, to trigger conversation. In particular, as @epage mention, there might be technical reasons to prefer one way or the other, and I'm fine with it.
> - As @brownjohnf mentioned, I completely left aside the question of find-like argument chaining. I think starting with sub-command chaining is the correct strategy, and it covers most use-cases in a way that, I believe, is more trendy by today's standards.
> - I kept using the `chain` terminology, for the lack of a better one. Although I agree with @brownjohnf that the use-case may or may not care about order, Clap should definitely keep the ordering properly, for those use-cases which need it (e.g. pipelines). `multiple` may be alternative, but it sound a bit too generic and is less explicit about the ordering aspect. I don't have another suggestion at this point.
> 


---

_Comment by @brownjohnf on 2023-03-30 18:30_

> - Should `prog chain0 --help` list all the chained commands like 
> `prog --help`?

I think it should function the same as if there were no chaining.

> - Should both levels of usage show all the chained commands?

I don't actually remember how this works today with subcommands, but I'd 
say the behavior should remain the same.

> - The parser for a chained command will need access to the parent 
> command (for command flags, knowing whether an arg is a command or 
> value) which will run into multiple borrows of a mutable value (`prog` 
> owns `chain0`, `chain0` will need to be mutable for the lazy building, 
> `prog` and `chain0` will both need to be accessible when parsing)

I'm not sure I'm following, but I'd assumed that this would be 
recursive, and the current context ((sub)command) wouldn't know about 
the parent. On failure or end of args it would just return whatever it 
did parse about itself + remaining args it didn't recognize. Then the 
parent command would resume attempting to parse it, which might recurse 
down into another subcommand, or might resume parsing flags belonging to 
that command.

> - In general, I'm leaning towards the recursive approach.  Its the 
> simplest, has no overhead for unchained subcommands (the more common 
> case), and the builder API doesn't need to be as ergonomic as that is 
> what the builder API is for

I think that makes sense to me.

>    - The main complication is the derive API knowing when to do this 
> (it'll have to capture the state of any chaining) and knowing when to 
> stop if we support unchained subcommands within a chained subcommand.

I'm not following this though. I'm pretty ignorant here, but I'd assume 
the derive API (I assume by that you mean the macro?) would just enable 
recursive parsing whenever it encountered a `Vec<T>`.

> - Subcommands under chained subcommands would get complicated if we 
> move forward with deferred initialization of `Command` because we can't 
> tell if subcommands are present until we build it, but we shouldn't 
> build it until after we've copied
> - This doesn't work for any call that wants to just do a 
> `prog.build()` as that shouldn't recurse infinitely

Also not following this, but I think it would be reasonable to say that 
you can't re-nest the same subcommands under themselves. i.e., your 
interface needs to be an acyclic graph. You can only have one subcommand 
field in any clap Command, and you can't have a subcommand `T` as be a 
subcommand of itself (including transitively).


---

_Comment by @epage on 2023-03-30 18:56_

> For this to work, nothing must happen besides showing help when --help is present, regardless of the existence of other, fully-formed sub-command.

Clap help behaves that way to do.

> Another aspect is sub-command grouping in the top-level help, as shown above. All of these commands are "sister" sub-command of the top-level CLI—no nesting or anything. Yet they appear thematically grouped. Click doesn't support it out-of-the-box and a stack overflow recipe is needed. It's simple enough, but it would be nice to have built-in support.

See
- https://github.com/clap-rs/clap/issues/1553
- https://github.com/clap-rs/clap/issues/4589

---

_Comment by @abey79 on 2023-03-30 19:06_

@brownjohnf I agree with your (hot) take and we may certainly lift that "no nesting of chained sub-command" rule. I've included it here because of my understanding that it simplified implementation *on Click's side*, but I understand things might be entirely different on Clap's side.

---

_Comment by @epage on 2023-03-30 19:08_

> I'd loosen this up a bit. A sub-command should eagerly parse all
arguments until failure, at which the parser should pop up the stack and
resume parsing the _parent_ (sub)command until failure, etc.

In thinking on chained subcommands, I realized that one challenge is clap is smarter than that allows.  At least for optional positionals and variable length positionals, clap checks for if a subcommand name exists among them and will then treat it as a subcommand.  At least for variable-length options, clap has a setting to check for subcommand names among them.  This requires subcommands to know every subsubcommand name and alias, without getting into subcommand short flags and long flags.

This presents an implementation challenge because
- clap's behavior should be consistent
- even if we accept chained commands being less than ideal now, it could be disruptive to change its behavior in the future

So we either need a solution that can have consistency out the gate or we need to be very restrictive and disallow any case that requires knowing about chained commands so we *can* just pop back up and continue processing.  I'm assuming that would be too restrictive for this to be feasible: we'd be giving users a taste of something that won't work in a lot of their cases and it'd be better to not have the feature in that case.

---

_Comment by @brownjohnf on 2023-03-30 19:47_

> At least for variable-length options, clap has a setting to check for
> subcommand names among them.  This requires subcommands to know every
> subsubcommand name and alias, without getting into subcommand short
> flags and long flags.

So you're saying that if I have subcommand foo, clap would extract that 
from the variable length args like:

```console
$ mycli -v --names bar foo

# Cli { verbose: true, names: [bar], subcommand: foo }
```

I think I understand the issue there. I'm not sure how to get around 
that with multi-subcommands short of propagating the knowledge, as you 
describe.

> be very restrictive and disallow any case that requires
> knowing about chained commands so we *can* just pop back up and
> continue processing.

What would that restriction look like? You mean stop allowing putting 
subcommands after variable length args? At least for myself, I'd rather 
have a single layer of variable length subcommands than none at all, but 
it does feel hacky to have that limitation.

Since we have no multi-subcommand support today, what about having a 
config option to enable it that disables the "smart" detection of 
subcommands after variable length args? That would be a way of opting 
into the feature without breaking any existing CLIs in the wild. Not 
ideal, but maybe a useful compromise? It would mean you had to avoid 
variable length args before subcommands, put them in as delimited flags 
using `Arg::value_delimiter`, or use a custom terminator with 
`Arg::value_terminator`.

Given that we're wrestling with this in general, is it worth at least 
pondering multi/positional flags as well? It's not clear to me if one is 
a superset of the other problem, or if they're entirely orthogonal.


---

_Comment by @epage on 2023-03-30 19:59_

> Since we have no multi-subcommand support today, what about having a
config option to enable it that disables the "smart" detection of
subcommands after variable length args?

This isn't an acceptable option
- Being a runtime setting means we'd be bloating the code and API for everyone for this less common feature
- We try to limit what gets a feature flag because those don't have an ideal workflow for end users
- As mentioned in my previous post, consistency is important.  You shouldn't be required to lose out on clap features to be able to use other features.

---

_Comment by @brownjohnf on 2023-03-30 20:03_

 >> Since we have no multi-subcommand support today, what about having a
 >> config option to enable it that disables the "smart" detection of
 >> subcommands after variable length args?
 >
 > This isn't an acceptable option

Okay, I think that's a helpful constraint. That means that we have no 
choice but to figure out a way for subcommands to have the necessary 
context to recognize sibling/parent options in addition to their own 
options, correct?

That does make me lean towards considering positional flags as part of 
this broader design effort, since that would also maybe be context that 
subcommands would need?

On 3/30/23 12:59, Ed Page wrote:
>> Since we have no multi-subcommand support today, what about having a
> config option to enable it that disables the "smart" detection of
> subcommands after variable length args?
> 
> This isn't an acceptable option
> - Being a runtime setting means we'd be bloating the code and API for everyone for this less common feature
> - We try to limit what gets a feature flag because those don't have an ideal workflow for end users
> - As mentioned in my previous post, consistency is important.  You shouldn't be required to lose out on clap features to be able to use other features.
> 


---

_Referenced in [clap-rs/clap#4813](../../clap-rs/clap/issues/4813.md) on 2023-03-30 23:47_

---

_Comment by @epage on 2023-03-31 15:21_

Something I realized is command chaining would get us a form of "argument chaining" (#1704)  for free because users could specify the start of an argument chain using `Command::long_flag`, making the chained subcommand act like an argument.  To clean up `--help`, they can specify do something like `cmd.subcommand_value_name("OP").subcommand_help_heading("Operation")`

---

_Comment by @muja on 2023-05-06 08:19_

I also have this use case. I am writing a tool for mass editing docx files. docx files are zip files with lots of XMLs and once unpacked and parsed, it is very desirable to perform multiple operations on the contents instead of spreading it out into multiple processes which would waste a lot of computation power.

The CLI should look something like this: `./docx-edit ~/Documents/WordFiles --output=/tmp/wordout replace --replace-pairs-file=ReplacePairs.xlsx append --paragraph-break append --text="© MyCompany 2015-2023"`
Here the subcommands are `replace` and `append`.

I would like to be able to use a `Vec<Subcommand>` in my Parser struct for ease of use. I also don't need nested subcommands for my use case.

---

_Comment by @frol on 2023-05-06 09:33_

I would like to also share my use case, where I need subcommand chaining with nested subcommands. I need to construct a complex structure from the command line, here is an extract:

```rust
struct Transaction {
    signer_account_id: String,
    actions: Vec<Action>,
}

enum Action {
    GrantAccess {
        account_id: String,
        permissions: Permissions,
    },
    Transfer {
        receiver_account_id: String,
        amount: u64,
    },
}

enum Permissions {
    FullAccess,
    LimitedAccess { allowed_actions: Vec<AllowedActionKind> },
}

enum AllowedActionKind {
    GrantAccess,
    Transfer,
}
```

Here is how I want the CLI to look like:

```
./cli construct-transaction // <- Top-level Commands
        "frol@github" // <- `signer_account_id: String`
        add-action // <- Either `add-action` or some "next" command, see `submit` below
            grant-access // <- Either `grant-access` or `transfer`
                "not-frol@github" // <- `account_id: String`
                limited-access // <- Either `full-access` without parameters or `limited-access` with allowed actions
                    transfer
                    grant-access
        add-action
            transfer // <- Either `grant-access` or `transfer`
                "not-frol@github" // <- `receiver_account_id: String`
                "123" // <- `amount: u64`
        add-action
            transfer // <- Either `grant-access` or `transfer`
                "not-frol2@github" // <- `receiver_account_id: String`
                "124" // <- `amount: u64`
        submit // <- Stop adding actions. Next subcommands should also be able to receive arguments and use nested subcommands
            https://github.com // <- Positional argument to `submit`
```

You may argue that nobody will write such a long command, and I totally agree, but in my case [near-cli-rs](https://github.com/near/near-cli-rs) has interactive mode which extends clap (there is [interactive-clap](https://github.com/FroVolod/interactive-clap) helper), and guides users through the available options with prompts, and then prints the final command to be reused later.

Currently, I copy-paste the code ([see `add_action_*` folders](https://github.com/near/near-cli-rs/tree/05e4acaceb8f5673bf72a480e4271473593a54e5/src/commands/transaction/construct_transaction)), but after 5 levels of nesting with 6 "actions" on each level there are too many variants ($6^5=7776$) and clap tries to walk through all of them, so it is not only ugly but also not scalable beyond a certain point.

The pattern I use there is basically:

```rust
struct ConstructTransaction {
    #[clap(subcommand)]
    next: AddActionOrSubmit_1,
}
```

And then there are copies of:

```rust
enum AddActionOrSubmit_1 {
    AddAction(AddAction),
    Submit(super::Submit),
}

struct AddAction {
    #[clap(subcommand)]
    action: Action,
}

enum Action {
    GrantPermissions(GrantPermissionsAction),
    Transfer(TransferAction),
}

...

struct TransferAction {
    receiver_account_id: String,
    amount: u64,
    #[clap(subcommand)]
    next: AddActionOrSubmit_2, // <- This is where "recursion" happens unless I manually unroll the code, and this is where it explodes when I add 5+ levels or unrolled copies
}
```

---

_Comment by @epage on 2023-05-06 13:40_

> clap tries to walk through all of them, so it is not only ugly but also not scalable beyond a certain point.

Unsure which aspect doesn't scale for you but #4792 will help clap scale with a lot of subcommands with a lot of arguments. For v4 it might have to be opt-in for derive users but then on by default for v5.

---

_Comment by @alexpovel on 2023-09-13 20:46_

In search for this issue/feature, first thing I actually found searching was [this thread](https://www.reddit.com/r/rust/comments/ci2r3d/multiple_subcommands_with_structoptclap/), where a user has an issue identical to mine (and everyone else's in this thread...):

> Does anyone know how to achieve the effect of parsing subcommands multiple times?
>
> I'd like to have a data processing tool where multiple operations can be performed on a stream of data/events/... and it should be possible to specify the steps via command line arguments like so:
>
> `magical_command capture --source=/dev/abcd filter --kind=large-ones normalise convert --to=jsonl`

I reckon that's more modest in comparison to other use cases in this thread. It'd be plenty for my uses anyway. Using a workaround, they [solved it as](https://www.reddit.com/r/rust/comments/ci2r3d/comment/ev2kso5/?context=3):

> I resorted to collecting `std::env::args()` into a `Vec<String>`, iterated over `.split("--")`, provided the zeroth argument (program name) at index 0 for each split and fed those to `MagicalArgs::from_iter`. It is bad in the sense that it abolutely requires the presence of `--` between subcommands.
>
> In hindsight, it is a rather obvious workaround that would have saved me half a day of frustration. Retains all the nice stuff of structopt and scales until the limits of linux command lines are reached. If only clap/structopt could parse partially and yield the remainder instead of failing with a hardcoded unkown argument error...

Could someone perhaps shed some light on this workaround: is it a viable one, while the present issue is in progress? Or is there a better approach? The [`find`](https://docs.rs/clap/latest/clap/_derive/_cookbook/find/index.html) and [`git`](https://docs.rs/clap/latest/clap/_derive/_cookbook/git/index.html) examples come close, but not quite.

---

_Comment by @muja on 2023-09-14 10:58_

I've actually independently resorted to the same workaround, the only difference being that I've used the word `and` as separator instead of (the apparently more common) `--`. I think it reads more nicely but has the downside of course that it implicitly blacklists `and` as file name argument etc. No downsides except that it's ~5-10 lines of set up in main.

So the command would look like this:

```
magical_command capture --source=/dev/abcd and filter --kind=large-ones and normalise and convert --to=jsonl
```

---

_Comment by @epage on 2023-11-11 03:00_

Trying to re-summarize click

### API

On the parent, set `chain = True`.  The built-in dispatcher will automatically call the appropriate functions
```python
@click.group(chain=True)
def cli():
    pass


@cli.command('sdist')
def sdist():
    click.echo('sdist called')


@cli.command('bdist_wheel')
def bdist_wheel():
    click.echo('bdist_wheel called')
```

### How it looks in help

**Parent command:** TBD

**Chained command:** TBD

### Behavior

- Cannot chain another subcommand after using one with variable number of arguments
- Cannot nest subcommands under chained subcommands
- Options must come before arguments

---

_Comment by @epage on 2023-11-11 03:01_

Trying to re-summarize and polish the proposal

### API

Setting it on the parent command is most likely the natural thing to do.

Parent commands will need to know it for the help.  When parsing child commands, we'll need to know its set on the parent.  This can be handled in our parsing book keeping.

Most likely this would end up looking like:
```rust
impl Command {
    pub fn chain_subcommands(self, yes: bool) -> Self
}
```

Likely the easiest way to implement this in `ArgMatches` will be to recurse but that will be harder for users, especially  `clap_derive` users.

Most likely this would end up looking like:
```rust
impl ArgMatches {
    pub fn subcommands(&self) -> impl Iterator<Item=(&str,, &ArgMatches)>;
    pub fn remove_subcommands(&mut self) -> impl Iterator<Item=(Str, ArgMatches)>;
    pub fn subcommands_present(&self) -> bool;
}
```
We'd likely keep the singular forms for ease of working with like we do for arguments.

We could probably deprecate `ArgMatches::subcommand_name` and `ArgMatches::subcommand_matches`

For derive users, I would expect this to translate to:
```rust
#[derive(Parser)]
struct Cli {
    #[command(subcommand)]
    #[command(subcommand_required = true, args_required_else_help = true)]
    subcommand: Vec<Command>,
}

enum Command {
    New,
    Build,
    Clean,
}
```
We'll consider the subcommand optional (zero-element `Vec`) and will require users to manually require it.  This will also match how we deal with positionals.

We can key off of the use of `Vec` and infer `chain_subcommands(true)`. 

### How it looks in help

**Parent command:**

No different aside from the usage.

```
Usage: code[EXE] [OPTIONS] <COMMAND> [OPTIONS] [<COMMAND> [OPTIONS]]...
```

I'm thinking that if `flatten_help(true)` is set (see #5206), then we'll the flattened usage, like normal, with `[<COMMAND> ...]` at the end of each line.  This would require knowing when they are chained.  We could pass that through as a flag but it would be helpful for chained command help

**Chained command:**

Either
- Only usage is affected
  - We'd need a way to know we are chained
- We show the chained subcommands as if they were our own subcommand
  - This requires being aware of a lot more state

### Behavior

Conceptually, the easiest is: when evaluating a potential subcommand, chain up.
- Anywhere a subcommand will work, a chained subcommand will work
  - Preserving existing use of names, long flags, and short flags
  - Preserving the existing precedence between arguments and subcommands
- We support arbitrary backtracing

In most cases, nested subcommands under a chained subcommand could be confusing and we'd likely want to steer people away from doing that.  However, using short/long flags for chained subcommands to create argument groups within chained subcommands would be a legitimate use case.  However, this isn't the highest priority and if it isn't in the initial release but can be added later, we won't block this feature on it.

Guarantees
- Repeating a subcommand is allowed
- Order of invocation is maintained


---

_Comment by @vallentin on 2024-02-25 11:37_

Many years later, I'm coming back to this issue. After finally wanting to convert all my hacky old `structopt` code into `clap`.  However, chaining adjacent commands was always the feature I needed, so `clap` still has to wait.

For the CLIs I'm working on, chaining commands is **the** thing I need. I fully understand, that there's many edge cases as mentioned in this thread. However, the CLIs I'm working on doesn't run into them. So now that I still can't use `clap`, I've looked through other major command-line parsers.

The ones I tested, that didn't support multiple adjacent commands are: [`clap`](https://crates.io/crates/clap), [`argh`](https://crates.io/crates/argh), and [`gumdrop`](https://crates.io/crates/gumdrop).

However, [`bpaf`](https://crates.io/crates/bpaf) does support multiple adjacent commands! A minimal example using `bpaf` looks like this:

```rust
use bpaf::Bpaf;

#[derive(Bpaf, Clone, Debug)]
#[bpaf(options)]
pub struct Options {
    #[bpaf(external, many)]
    cmd: Vec<Cmd>,
}

#[derive(Bpaf, Clone, Debug)]
pub enum Cmd {
    #[bpaf(command, adjacent)]
    Foo { a: bool },
    #[bpaf(command, adjacent)]
    Bar { b: bool },
    #[bpaf(command, adjacent)]
    Baz { c: bool },
}

fn main() {
    let opt = options().run();
    println!("{:#?}", opt);
}
```

Which then when executing:

```shell
[program] foo -a bar bar -b baz -c baz
```

Outputs the following:

```rust
Options {
    cmd: [
        Foo {
            a: true,
        },
        Bar {
            b: false,
        },
        Bar {
            b: true,
        },
        Baz {
            c: true,
        },
        Baz {
            c: false,
        },
    ],
}
```

That being said, I only have mere minutes of experience using `bpaf`. So I don't know the _real_ differences between e.g. `clap` and `bpaf`. All I can say is, I previously had multiple projects, with semi-complex CLIs using `structopt` and `clap`, and all of them _easily_ mapped into `bpaf`, with no issues so far. But again, take it with a grain of salt.

I'm not advocating the use of `bpaf` instead of `clap`. I personally wanted to use `clap`. However, supporting multiple adjacent commands, was **the** feature I needed. So I have to go with `bpaf`.


---

_Comment by @epage on 2024-02-26 17:20_

> After finally wanting to convert all my hacky old structopt code into clap. However, chaining adjacent commands was always the feature I needed, so clap still has to wait.

I'm not aware of `structopt` supporting this, so I'm confused how this would be a blocker for moving off of it.

---

_Comment by @vallentin on 2024-02-27 01:25_

@epage I'm confused why that's the take away. Anyways, I initially preferred `structopt` because of its derive macro, obviously today that has changed. However, as I said my solution was hacky. In short, instead of using [`from_args()`](https://docs.rs/structopt/latest/structopt/trait.StructOpt.html#method.from_args), I would instead use [`from_iter()`](https://docs.rs/structopt/latest/structopt/trait.StructOpt.html#method.from_iter) and manually split the args beforehand based on a delimiter. Again, like I said, hacky solution. But it allowed me to somewhat simulate having multiple chained adjacent commands.

---

_Comment by @epage on 2024-02-27 01:48_

Thanks for the clarification.  That wasn't meant to be a "take away" but it was a leap that wasn't sufficient on its own with what was said so I was wanting to better understand.



---

_Comment by @vallentin on 2024-02-27 11:53_

@epage if you're curious about the use case, I have various tools that operate on data or allow performing multiple actions sequentially.

```shell
[program] [--dry-run] \
    [cmd] [cmd-options...] \
    [cmd] [cmd-options...] \
    [cmd] [cmd-options...]
```

Actual example for rendering 2 images:

```shell
draw \
    size 100 100 \
    fill  0  0 50 50 "#FF0000" \
    fill 50  0 50 50 "#00FF00" \
    fill  0 50 50 50 "#0000FF" \
    fill 50 50 50 50 "#FFFFFF" \
    export --output "image.png" \
    fill 25 25 50 50 "#FFFF00" \
    export --output "image2.png" \
```

In reality, I could create config formats instead. However, many times I'm executing a single command, so having to create a config is tedious, e.g.:

```shell
draw --input "image.png"  --output "image-inverted.png" \
    invert  
```

Additionally, both supporting a CLI and config doubles the code. So many times, I end up with a CLI and then create a `.sh` file when I need a "config".

---

_Comment by @abey79 on 2024-02-27 11:57_

I can second this exact use case. I'm considering a rust rewrite of [vpype](https://github.com/abey79/vpype), which is very similar to @vallentin's example (albeit targeted at vector graphics for the plotter-based art niche).

Example:

```
vpype  read input.svg  layout --landscape --fit-to-margin 1.5cm a4  linesort --tolerance 0.1mm  write output.svg
```

---

_Referenced in [clap-rs/clap#1704](../../clap-rs/clap/issues/1704.md) on 2024-03-21 12:34_

---

_Referenced in [abey79/vsvg#153](../../abey79/vsvg/pulls/153.md) on 2024-09-22 12:51_

---

_Comment by @ahirner on 2024-12-06 20:50_

I think many cases can be solved with `.defer()` quite generically. 

Setup:
```rust
#[derive(Debug, Parser)]
struct Cli {
    #[clap(long)]
    top: String,
    #[command(subcommand)]
    cmd: Option<Cmd>,
}

#[derive(Debug, Subcommand)]
#[command(subcommand_precedence_over_arg = true)]
enum Cmd {
    Foo(ReClap<FooOpts, Self>),
    Bar(ReClap<BatOpts, Self>),
    Baz(ReClap<BatOpts, Self>),
}

#[derive(Debug, Args)]
struct FooOpts {
    #[clap(long, short('a'))]
    ayy: Vec<usize>,
    #[clap(long)]
    hey: Option<String>,
}

/// Bar/Baz Docstring.
#[derive(Debug, Args)]
struct BatOpts {
    #[clap(long)]
    ok: bool,
}


fn main() {
    let cli = Cli::parse_from("main --top 8 baz --ok foo -a 1 -a 2 bar --ok baz".split(' '));
    let cli = Cli::parse();

    println!("top: {}", &cli.top);
    let mut next = cli.cmd;
    while let Some(cmd) = next {
        println!("{cmd:?}");
        // could use enum_dispatch
        next = match cmd {
            Cmd::Foo(rec) => (rec.next).map(|d| *d),
            Cmd::Bar(rec) => (rec.next).map(|d| *d),
            Cmd::Baz(rec) => (rec.next).map(|d| *d),
        }
    }
}
```

Examples:
```
$ cargo r -- --top 8 baz --ok foo -a 1 -a 2 bar --ok baz
top: 8
Baz(ReClap { inner: BatOpts { ok: true }, next: Some(Foo(ReClap { inner: FooOpts { ayy: [1, 2], hey: None }, next: Some(Bar(ReClap { inner: BatOpts { ok: true }, next: Some(Baz(ReClap { inner: BatOpts { ok: false }, next: None })) })) })) })
Foo(ReClap { inner: FooOpts { ayy: [1, 2], hey: None }, next: Some(Bar(ReClap { inner: BatOpts { ok: true }, next: Some(Baz(ReClap { inner: BatOpts { ok: false }, next: None })) })) })
Bar(ReClap { inner: BatOpts { ok: true }, next: Some(Baz(ReClap { inner: BatOpts { ok: false }, next: None })) })
Baz(ReClap { inner: BatOpts { ok: false }, next: None })

$ cargo r -- --top 8 baz --ok foo --help
Usage: clap-ext-play baz foo [OPTIONS] [COMMAND]

Commands:
  foo
  bar  Bar/Baz Docstring
  baz  Bar/Baz Docstring

Options:
      --ayy <AYY>
      --hey <HEY>
  -h, --help       Print help
```

Implementation:
```Rust
/// `[Args]` wrapper to match `T` variants recursively in `U`.
#[derive(Debug, Clone)]
pub struct ReClap<T, U>
where
    T: Args,
    U: Subcommand,
{
    /// Specific Variant.
    pub inner: T,
    /// Enum containing `Self<T>` variants, in other words possible follow-up commands.
    pub next: Option<Box<U>>,
}

impl<T, U> Args for ReClap<T, U>
where
    T: Args,
    U: Subcommand,
{
    fn augment_args(cmd: clap::Command) -> clap::Command {
        T::augment_args(cmd).defer(|cmd| U::augment_subcommands(cmd.disable_help_subcommand(true)))
    }
    fn augment_args_for_update(_cmd: clap::Command) -> clap::Command {
        unimplemented!()
    }
}

impl<T, U> FromArgMatches for ReClap<T, U>
where
    T: Args,
    U: Subcommand,
{
    fn from_arg_matches(matches: &clap::ArgMatches) -> Result<Self, clap::Error> {
        //dbg!(&matches);
        // this doesn't match subcommands but a first match
        let inner = T::from_arg_matches(matches)?;
        let next = if let Some((_name, _sub)) = matches.subcommand() {
            //dbg!(name, sub);
            // most weirdly, Subcommand skips into the matched
            // .subcommand, hence we need to pass outer matches
            // (which in the average case should only match enumerated T)
            Some(U::from_arg_matches(matches)?)
            // we are done, since sub-sub commmands are matched in U::
        } else {
            None
        };
        Ok(Self { inner, next: next.map(Box::new) })
    }
    fn update_from_arg_matches(&mut self, _matches: &clap::ArgMatches) -> Result<(), clap::Error> {
        unimplemented!()
    }
}
...
```
[Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=07c2cef796ee6df98669e5bc0eaf0288).

I see no practical length limit of the command chain, although there are quirks for sure.
I'm curious how it could solve anybody's problem.

---

_Referenced in [near/near-cli-rs#408](../../near/near-cli-rs/issues/408.md) on 2024-12-08 10:42_

---

_Comment by @willchet on 2024-12-20 17:26_

I stumbled across this issue within a bioinformatics use-case. We are attempting to write a sequence trimmer which can support primer trimming, barcode trimming, adapter trimming, and hard trimming (each of which would ideally be a separate subcommand). Allowing multiple of these to be chained together would allow for us to achieve good flexibility and efficiency.

Given that a solution isn't present in Clap yet, do you have any recommendations for work-arounds @epage, or what would be most "idiomatic" within Clap? Some ideas include:
* Support at most four chained commands in a way similar to [here](https://github.com/near/near-cli-rs/issues/408), where there are separate structs representing the first action, which is optionally followed by the second action, etc.
* Use `trailing_var_arg` or something similar to obtain a `Vec<String>`, and then use a while loop to keep reparsing it until it is empty.

/cc @sammysheep @samcwiley

---

_Comment by @epage on 2024-12-20 17:33_

Thats a question of trade offs.  The first will give you better error messages while the second gives more flexibility.

---

_Referenced in [YaLTeR/niri#914](../../YaLTeR/niri/issues/914.md) on 2025-01-12 21:09_

---

_Referenced in [clap-rs/clap#5935](../../clap-rs/clap/issues/5935.md) on 2025-03-03 15:38_

---

_Comment by @sjwo on 2025-03-06 21:29_

Use case:

My dissertation research involves code for testing various state space search algorithms against instances from various problem domains. A call always specifies one algorithm, and one instance from a given problem domain. Each algorithm has a different set of parameters, as does each domain.

Ideally I could write
```
#[derive(Parser)]
struct Cli {
    #[command(subcommand)]
    algorithm: Algorithm,
    #[command(subcommand)]
    domain: Domain,
}

enum Algorithm { ... }
enmu Domain { ... }
```
with arbitrarily complex enum variants for each algorithm and each domain.

---

_Referenced in [clap-rs/clap#6026](../../clap-rs/clap/issues/6026.md) on 2025-06-09 14:22_

---

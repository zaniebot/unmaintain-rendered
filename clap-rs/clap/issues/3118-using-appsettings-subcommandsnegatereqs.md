---
number: 3118
title: "Using AppSettings::SubcommandsNegateReqs"
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:13:16Z
updated_at: 2021-12-09T16:41:04Z
url: https://github.com/clap-rs/clap/issues/3118
synced_at: 2026-01-10T01:27:33Z
---

# Using AppSettings::SubcommandsNegateReqs

---

_Issue opened by @epage on 2021-12-09 16:13_

<a href="https://github.com/erickpires"><img src="https://avatars.githubusercontent.com/u/6362864?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [erickpires](https://github.com/erickpires)**
_Friday Jul 13, 2018 at 19:26 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/124_

----

I'm sorry if I'm missing something here, but I was unable to find any information on the documentation or in the examples.

I have a use case where my program has a required Arg (for example a input file name). I would like to implement shell completions generation using clap during runtime by passing a `completions`subcommand and the shell to generate the completions for (the same way `rustup` does). For this to happen I need to set `AppSettings::SubcommandsNegateReqs`. Using the examples as a guide I did this:

```
#[structopt(..., raw(global_settings = "&[AppSettings::SubcommandsNegateReqs]")]
```

But now, when I run `cargo run completions zsh` for example, my program panics at `Opt::from_args()` due to a `None` been `unwrap`'d.

I've found a workaround this by getting the clap matches via `Opt::clap().get_matches()` and matching against the subcommands (and exiting early if the subcommand was found), but I found this very ugly and was thinking that there must be a clean way to do this using `structopt`.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Jul 13, 2018 at 20:06 GMT_

----

I didn't know this command. But what behavior do you expect, because `from_args` can't work in this case.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/erickpires"><img src="https://avatars.githubusercontent.com/u/6362864?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [erickpires](https://github.com/erickpires)**
_Monday Jul 16, 2018 at 20:27 GMT_

----

I understand that `from_args` can't work in this situation. When using `clap` to implement subcommands that should negate required arguments I use the [`subcomand()`](https://docs.rs/clap/2.32.0/clap/struct.ArgMatches.html#method.subcommand) from `ArgsMatches`, so I think ideal solution here would be to try and replicate this behavior.

My thinking is, since I have a `Subcommands` enum that lists all the possible subcommands and my `Opt` struct has a field of type `Subcommand`, we could have something like `Opt::subcommand()` that would return an `Option<Subcommands>`. This way I can `match` against this, similar to what I would do when using `clap` and I know to only call `from_args` when the return is `None`.

Now, what I don't know is if this is even possible. I don't know what we can and cannot do with `#[derive]`. I would be happy to help implement this, but I would like to know your opinion about this. 


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Jul 17, 2018 at 08:01 GMT_

----

@kbknapp what do you think about that?


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/sopium"><img src="https://avatars.githubusercontent.com/u/6764397?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [sopium](https://github.com/sopium)**
_Monday Sep 30, 2019 at 11:56 GMT_

----

I run into this issue as well. Here's my workaround: define an almost identical struct but make the fields optional, and use it to access match result.

```rust
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct OptionsAndSubcommand {
    #[structopt(long)]
    arg: i32,

    #[structopt(subcommand)]
    cmd: Option<Cmd>,
}

#[derive(Debug, StructOpt)]
enum Cmd {
    Get {
        name: String,
    },
}

#[derive(Debug, StructOpt)]
struct OptionalOptions {
    // Proper support should allow something like:
    // #[structopt(required_unless_subcommand)]
    arg: Option<i32>,

    #[structopt(subcommand)]
    cmd: Option<Cmd>,
}

fn main() {
    let matches = OptionsAndSubcommand::clap()
        .setting(structopt::clap::AppSettings::SubcommandsNegateReqs)
        .get_matches();

    println!("Options: {:?}", OptionalOptions::from_clap(&matches));
}
```

If you don't expect any args when there is a subcommand, you can do this:
```rust
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct OptionsAndSubcommand {
    #[structopt(long)]
    arg: i32,

    #[structopt(subcommand)]
    cmd: Option<Cmd>,
}

#[derive(Debug, StructOpt)]
enum Cmd {
    Get {
        name: String,
    },
}

fn main() {
    let matches = OptionsAndSubcommand::clap()
        .setting(structopt::clap::AppSettings::SubcommandsNegateReqs)
        .setting(structopt::clap::AppSettings::ArgsNegateSubcommands)
        .get_matches();

    if matches.subcommand_name().is_none() {
        println!("Options: {:?}", OptionsAndSubcommand::from_clap(&matches));
    } else {
        println!("Subcommand: {:?}", Cmd::from_clap(&matches));
    }
}
```



---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/keturn"><img src="https://avatars.githubusercontent.com/u/83819?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [keturn](https://github.com/keturn)**
_Tuesday Dec 24, 2019 at 07:15 GMT_

----

First I tried 
```rust
    #[structopt(required=true)]
    arg: Option<i32>,
```

but structopt complained I was contradicting myself.

So I was somewhat surprised to discover that this seems to work:

```rust
    #[structopt(required_unless="cmd")]
    arg: Option<i32>,
```

Not ideal, since it requires duplicating the name of the subcommand  (or names, using `required_unless_one`), but it's a workaround.

Could structopt ease up on that "required is meaningless for Option" error when SubcommandsNegateReqs is on? Perhaps not where that error is raised currently, it doesn't look like that has any way to see what the app settings are.

Maybe some kind of `allow` mechanism to override that error? It is, after all, safer to wrap a "required" value in an Option than it is when I shoot myself in the foot by passing `required=false` on a non-Option field.

Or could that validation be done later, when it's known what the app settings are? (maybe even at runtime, if necessary)


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Jan 07, 2020 at 20:46 GMT_

----

We can remove the error and do a warning, with some mechanism to remove the warning.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Jan 08, 2020 at 05:10 GMT_

----

There's no way to issue warnings on stable.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/archer884"><img src="https://avatars.githubusercontent.com/u/679494?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [archer884](https://github.com/archer884)**
_Wednesday Apr 22, 2020 at 19:59 GMT_

----

> ```rust
> struct OptionalOptions {
>     // Proper support should allow something like:
>     // #[structopt(required_unless_subcommand)]
>     arg: Option<i32>,
> 
>     #[structopt(subcommand)]
>     cmd: Option<Cmd>,
> }
> ```

I'm not sure that's the way I'd want to go. Ideally, what I had in mind was:

```rust
enum Opt {
    #[structopt(default_command)]
    MainCommandWithLotsOfArgs {
        ...
    },
    Other,
    Subcommands,
    Here { some_arg: i32 },
}
```

The reason for this being just that the main command may involve lots of args that you don't feel like marking separately. At this point, of course, I'm just spitballing about ergonomics. I'm coming at this from the standpoint of a consumer, not a library author.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Apr 22, 2020 at 20:15 GMT_

----

> I'm not sure that's the way I'd want to go. Ideally, what I had in mind was:
> 
> ```rust
> enum Opt {
>     #[structopt(default_command)]
>     MainCommandWithLotsOfArgs {
>         ...
>     },
>     Other,
>     Subcommands,
>     Here { some_arg: i32 },
> }
> ```
> 
> The reason for this being just that the main command may involve lots of args that you don't feel like marking separately. At this point, of course, I'm just spitballing about ergonomics. I'm coming at this from the standpoint of a consumer, not a library author.

Actually, this design looks neat and unambigous! 👌 If @TeXitoi  doesn't mind, would you like to take a stab at implementation? I can mentor it and give you a hand.

> I'm just spitballing about ergonomics. I'm coming at this from the standpoint of a consumer, not a library author. 

It is always about ergonomics to some extend. Does stuff feel intuitive enough (define intuitive, aha), does it meld into other parts of the library, etc. The more people come and propose ideas, the better the chances that something worthy will eventually be found. I personally thing your proposal is great.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/archer884"><img src="https://avatars.githubusercontent.com/u/679494?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [archer884](https://github.com/archer884)**
_Wednesday Apr 22, 2020 at 20:17 GMT_

----

@CreepySkeleton I'd be cool with that.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Tuesday Apr 28, 2020 at 10:36 GMT_

----

@TeXitoi What do you think about @archer884 design above?


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Apr 28, 2020 at 20:27 GMT_

----

Not found of `default_command`, we van fond a better name.

It must work
 - within a structure for common args, and without for arg less subcommands.
 - with an embedded struct enum, and with a 1-uple enum containing a struct.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/archer884"><img src="https://avatars.githubusercontent.com/u/679494?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [archer884](https://github.com/archer884)**
_Tuesday Apr 28, 2020 at 21:12 GMT_

----

@TeXitoi, regarding the first bullet point: if the purpose of this is to make it possible *not* to have common args between all subcommands, how much value lies in making it work within a struct defining common args for subcommands?

I could be somewhat biased. As a user of this library (I think practically all of my tools are based on structopt), I don't think I have ever intentionally made use of the common arguments feature. To avoid rewriting the same code from subcommand to subcommand, my preferred strategy has been to define common arguments as a struct and `#[structopt(flatten)]` those into the parent structs of each subcommand. This avoids what I see as the somewhat confusing pattern of `cmd --param arg subcommand --param otherarg`. (Admittedly, Microsoft follows this pattern for their `dotnet` cli tool, but that's one of my biggest complaints about their tool.)

An example:

```rust
#[derive(Clone, Debug, StructOpt)]
enum SubCommands {
    Foo(Foo),
    Bar(Bar),
}

#[derive(Clone, Debug, StructOpt)]
struct Foo {
    #[structopt(flatten)]
    common_args: CommonArgs,
    other: String,
    args: String,
}

#[derive(Clone, Debug, StructOpt)]
struct Bar {
    #[structopt(flatten)]
    common_args: CommonArgs,
    other: i32,
    args: String,
}

#[derive(Clone, Debug, StructOpt)]
struct CommonArgs {
    name: String,
    date: String,
}
```


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Apr 28, 2020 at 21:28 GMT_

----

The goal is to be consistent: you can without this feature, so this feature must not be an exception.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Apr 29, 2020 at 13:18 GMT_

----

Guys, I suspect you're speaking past each other here.

Regarding the first point, @TeXitoi could you please elaborate? A bit of code example would help immensely!

The second: @TeXitoi was talking about 
```rust
enum Opt {
    #[structopt(default_command)]
    MainCommandWithLotsOfArgs(MainApp),
    Other,
    Subcommands,
    Here { some_arg: i32 },
}

struct MainApp { ... }
```

Super easy to implement, about +10 LOC.


---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/archer884"><img src="https://avatars.githubusercontent.com/u/679494?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [archer884](https://github.com/archer884)**
_Wednesday Apr 29, 2020 at 13:25 GMT_

----

Oh. I thought that was what I proposed in the first place.
On Apr 29, 2020, 8:22 AM -0500, CreepySkeleton <notifications@github.com>, wrote:
> Guys, I suspect you're speaking past each other here.
> Regarding the first point, @TeXitoi could you please elaborate? A bit of code example would help immensely!
> The second: @TeXitoi was talking about
> enum Opt {
>    #[structopt(default_command)]
>    MainCommandWithLotsOfArgs(MainApp),
>    Other,
>    Subcommands,
>    Here { some_arg: i32 },
> }
>
> struct MainApp { ... }
> Super easy to implement, about +10 LOC.
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub, or unsubscribe.



---

_Comment by @epage on 2021-12-09 16:13_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Wednesday Apr 29, 2020 at 17:48 GMT_

----

```rust
enum Opt {
    #[structopt(default_command)]
    MainCommandWithLotsOfArgs(MainApp),
    Other,
    Subcommands,
    Here { some_arg: i32 },
}
struct BigOpt {
    #[structopt(subcommand)] 
    cmd: Opt,
    #[structopt(long, short)]] 
   config: String,
}
```
StructOpt on Opt and on BigOpt should work as expected (i.e. config always accessible).


---

_Comment by @epage on 2021-12-09 16:20_

SubcommandsNegateReqs now works, see #2255 

Let me know if I missed any other details in this thread but otherwise, I'm closing it.

---

_Closed by @epage on 2021-12-09 16:20_

---

_Label `A-derive` added by @epage on 2021-12-09 16:41_

---

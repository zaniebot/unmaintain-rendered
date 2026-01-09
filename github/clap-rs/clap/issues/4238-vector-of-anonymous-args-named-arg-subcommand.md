---
number: 4238
title: "Vector of anonymous args + named arg + subcommand leads to USAGE which fails to parse with `cargo run ... --`"
type: issue
state: closed
author: jacg
labels:
  - C-bug
assignees: []
created_at: 2022-09-20T12:07:42Z
updated_at: 2022-09-22T18:53:43Z
url: https://github.com/clap-rs/clap/issues/4238
synced_at: 2026-01-07T13:12:20-06:00
---

# Vector of anonymous args + named arg + subcommand leads to USAGE which fails to parse with `cargo run ... --`

---

_Issue opened by @jacg on 2022-09-20 12:07_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

{ version = "3.2.20", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(clap::Parser)]
#[clap(name = "erm", about = "Doesn't accept NAMED before ANONYMOUS as advertised by USAGE")]
pub struct Cli {

    // Changing this from Vec<u32> to u32 makes the problem go away
    pub anonymous: Vec<u32>,

    #[clap(short, long)]
    pub named: u32,

    // Removing this makes the problem go away
    #[clap(subcommand)]
    reco: Subcommand,
}

#[derive(clap::Subcommand)]
enum Subcommand {
    SubA,
    SubB,
}

fn main() {
    Cli::parse();
    println!("All's well");
}

```


### Steps to reproduce the bug with the above code

`cargo run --bin erm -- --named 1 2 sub-a`

### Actual Behaviour


1. `target/debug/erm 1 --named 2 sub-a` -> All's well
2. `target/debug/erm --named 1 2 sub-a` -> All's well
3. `cargo run --bin erm -- 1 --named 2 sub-a` -> All's well
4. `cargo run --bin erm -- --named 1 2 sub-a` fails

By 'fails' I mean that it displays the `clap` help message, because it fails to parse the command line.


### Expected Behaviour

Version 4, above, shoud behave just like versions 1-3.

In other words, it should manage to parse the command line, which

+ is the same as the one in version 2, but in the context of `cargo run`
+ agrees with the `clap`-generated USAGE: `erm --named <NAMED> [ANONYMOUS]... <SUBCOMMAND>`

### Additional Context

This behaviour was observed in both `zsh` and `bash`. I'm aware that `zsh` has some options which may affect how commands are parsed, but don't remember any details about this.

### Debug Output

_No response_

---

_Label `C-bug` added by @jacg on 2022-09-20 12:07_

---

_Comment by @epage on 2022-09-20 16:44_

With clap 3.2.22 and bash, Case 2 matches Case 4 in erroring out for me.

---

_Comment by @epage on 2022-09-20 16:49_

The reason `cargo run --bin erm -- --named 1 2 sub-a` fails is `2` and `sub-a` are being consumed into `anonymous`.  In clap 3, the derive defaults to `SubcommandRequiredElseHelp`.  Since no subcommand was found, it then shows the help.

In clap 4, we are switching the derive to `subcommand_required(true).arg_required_else_help(true)` which will show the help if you just run `erm` but will then show the subcommand-required error for Case 4, making what is happening more clear.

If you want the subcommand to win out over `anonymous`, you need to set `subcommand_precedence_over_arg `, like
```rust
use clap::Parser;

#[derive(clap::Parser)]
#[clap(
    name = "erm",
    about = "Doesn't accept NAMED before ANONYMOUS as advertised by USAGE",
    subcommand_precedence_over_arg = true
)]
pub struct Cli {
    // Changing this from Vec<u32> to u32 makes the problem go away
    pub anonymous: Vec<u32>,

    #[clap(short, long)]
    pub named: u32,

    // Removing this makes the problem go away
    #[clap(subcommand)]
    reco: Subcommand,
}

#[derive(clap::Subcommand)]
enum Subcommand {
    SubA,
    SubB,
}

fn main() {
    Cli::parse();
    println!("All's well");
}
```

---

_Comment by @epage on 2022-09-20 16:50_

See also https://github.com/clap-rs/clap/issues/3721

---

_Comment by @epage on 2022-09-20 16:51_

Looking closer at https://github.com/clap-rs/clap/issues/3721, these seem to be the same thing, closing this out in favor of the other issue.  If there is any concerns not directly related to https://github.com/clap-rs/clap/issues/3721, let us know and we can re-open.

---

_Closed by @epage on 2022-09-20 16:51_

---

_Comment by @jacg on 2022-09-20 18:33_

Thanks. Agree with closing this in favour of #3721.

The `subcommand_precedence_over_arg` solution works as proposed.

Is it possible to make `anonymous` be displayed before `named` in the help? `display_order` seems to be intended for positional arguments only.




---

_Comment by @epage on 2022-09-20 22:11_

display order is meant to change the other within a group and does't affect the ordering of groups.

I don't think its worth changing the default as this is the order people generally expect.  I don't see it being worth the cost to customize it (API surface, code size, etc).  It also doesn't help that I'm not sympathetic to a case where positional arguments and subcommands are being mixed; that just seems like its asking for trouble, in terms of user confusion if nothing else.

---

_Comment by @jacg on 2022-09-21 06:49_

Perfectly reasonable.

What design would you suggest for the use case

+ an arbitrary (often very large) number of input files (`anonymous` in the earlier example)
+ exactly one output file (`named`)
+ exactly one obligatory choice of algorithm (the subcommand)

?

I was hoping for something like `prog input1 input2 input3 --out output algo` (which, using your `subcommand_precendence_over_arg` hint, now works). I think that is preferable to `prog --out output input1 input2 input3 algo` which (also works and) is what `clap` suggest in `USAGE`.

As the input files typically number in the hundreds (and are specified with a wildcard) I'd rather avoid having to prepend each and every input with a `-i` which AFAICT would be mandatory if the inputs weren't anonymous/positional.

(Apologies if this is not an appropriate place to ask this question. If so, just say so, and I'll buzz off.)

---

_Comment by @epage on 2022-09-21 16:54_

Why is the algorithm a subcommand?  And why are the args coming before it?


Normally, I would all relevant arguments to come *after* the subcommand.  Any arguments that come before the subcommand are generally application-wide configuration (color and logging support, etc).

---

_Comment by @jacg on 2022-09-22 08:07_

> Why is the algorithm a subcommand?

Hmm, maybe because of a combination of historical reasons and misunderstandings that have been lost in the mists of time ...

> And why are the args coming before it?

The interface that we've got used to looks something like this:

`prog      data you want to process      --out where-result-should-be-written       processing-algorithm algo-arg1 algo-arg2`

Probably because, historically, we wanted to compare how different `processing-algorithm`s treated the same data, and hitting `<up>` (or `C-p` or whatever) in the shell and editing the end of the line (where the cursor is by default) was most convenient.

> Normally, I would all relevant arguments to come after the subcommand.

In our case, these would be the args to `processing-algo`. (Admittedly, we're still running all our `processing-algorithm`s with hard-wired parameters, so none of them take any arguments, *yet* [edit: that is incorrect, there are some, I haven't used them for so long that I forgot], so maybe there's something I'm overlooking lurking in that corner. )

> Any arguments that come before the subcommand are generally application-wide configuration (color and logging support, etc).

As this is code for analyzing experimental data, and not your typical 'application', I guess it doesn't fit comfortably into the usual mould. Colour, logging and other typical application-config are completely irrelevant in this case.

In an exploratory phase, the data are fairly constant: you want to see how different algos treat some representative data. So the input data are constant, and should go at the front: you want to try different algos and tweak their parameters, so those should go at the end, where they are most easily editable in the shell.

Once you've discovered a good algo with a decent set of parameters, you probably want to fix those and run the process on many different sets of data. At this point the algo and its parameters feels like it should go at the front and the data at the end.

But, as I said, this might be a misguided consequence of history and misunderstandings.

---

_Comment by @epage on 2022-09-22 18:18_

Why isn't the algorithm a required option (`--algorithm foo`)?

---

_Comment by @jacg on 2022-09-22 18:36_

Quite possibly because I'm completely missing something.

For the sake of illustration imagine we have two algorithms:

1. `foo`, with optional arguments `this` and `that`
2. `bar`, with optional arguments `ho`, `hum` and `hmmm`

How would you express that with the algorithms being required options?


---

_Comment by @epage on 2022-09-22 18:40_

If you do a `--foo` flag, you could put `--this` and `--that` in a group that requires `--foo`.  Unfortunately, I don't think we can setup that relationship today for `--algorithm foo`.

---

_Comment by @jacg on 2022-09-22 18:48_

I should mention that we've recently migrated from `structopt`. I recall that I originally had some trouble with getting groups to do what I wanted in `structopt`. Does `derive` provide the full feature set of groups?

---

_Comment by @epage on 2022-09-22 18:53_

The feature set should be similar.

---

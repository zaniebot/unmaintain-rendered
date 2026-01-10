---
number: 4980
title: Implement Subcommand when deriving Parser for a struct that has a Subcommand field
type: issue
state: closed
author: Heliozoa
labels:
  - C-enhancement
assignees: []
created_at: 2023-06-21T21:19:45Z
updated_at: 2023-06-22T01:46:32Z
url: https://github.com/clap-rs/clap/issues/4980
synced_at: 2026-01-10T01:28:04Z
---

# Implement Subcommand when deriving Parser for a struct that has a Subcommand field

---

_Issue opened by @Heliozoa on 2023-06-21 21:19_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

I have a Subcommand enum and I'd like to add an argument that is used with all of them.

### Describe the solution you'd like

Normally in Rust, when you have a field used with all variants of an enum it's common to define a struct that has the enum as an inner field. Something like so:

```rs
struct MyType {
    common_field: String,
    kind: MyTypeKind,
}

enum MyTypeKind {
    A,
    B {
        some_field: String
    },
    // ..
}
```

I think it would be nice if this same pattern could be used with clap like this:

```rs
#[derive(Parser)]
pub struct Cmd {
    #[clap(long)]
    common_arg: String,
    // "forwards" the subcommand implementation to this field that implements Subcommand
    // a struct needs to have a field with this annotation for the derive to impl Subcommand
    #[clap(bikeshed)]
    kind: CmdKind,
}

#[derive(Parser)]
pub enum CmdKind {
    A,
    B {
        #[clap(long)]
        some_arg: String,
    },
    // ..
}
```

This would behave the same as if `Cmd` was `CmdKind` with `common_arg` as a field in every variant.



### Alternatives, if applicable

I'm aware of two alternative solutions:
Adding the argument to each variant manually. This makes the argument cumbersome to maintain and access.
Moving the argument "up" one level. This makes it more convenient for the developer, but makes the CLI's user experience worse, as the placement of the arg when using the CLI or reading the help output is off.

### Additional Context

A potential issue with the suggested solution is that a struct and one of its subcommand variants could have arguments with the same names. I'm not sure how clap deals with conflicts like that in general, hopefully it's not a blocker if this feature is otherwise desireable.

When searching for related issues I got hundreds of results even when trying to use more specific wording, sorry if I missed an existing one.

---

_Label `C-enhancement` added by @Heliozoa on 2023-06-21 21:19_

---

_Closed by @Heliozoa on 2023-06-21 21:31_

---

_Comment by @Heliozoa on 2023-06-21 21:36_

My bad, it seems like if `Cmd` is a struct I can just leave out the `#[clap(subcommand)]` annotation and it'll work as I'd like it to.

Previously I had something like
```rs
#[derive(Debug, Parser, Clone)]
struct Cli {
    #[clap(long)]
    arg: String,
    #[clap(subcommand)]
    cmd: Cmd,
}

#[derive(Debug, Parser, Clone)]
enum Cmd {
    #[clap(subcommand)]
    MySubcommand(MySubcommandEnum),
}

#[derive(Debug, Parser, Clone)]
enum MySubcommandEnum {
    A,
    B { some_arg: String },
}
```
and now
```rs
#[derive(Debug, Parser, Clone)]
struct Cli {
    #[clap(long)]
    arg: String,
    #[clap(subcommand)]
    cmd: Cmd,
}

#[derive(Debug, Parser, Clone)]
enum Cmd {
    // no #[clap(subcommand)] here
    MySubcommand(MySubcommandStruct),
}

#[derive(Debug, Parser, Clone)]
struct MySubcommandStruct {
    #[clap(long)]
    common_arg: String,
    #[clap(subcommand)]
    kind: MySubcommandEnum,
}

#[derive(Debug, Parser, Clone)]
enum MySubcommandEnum {
    A,
    B { some_arg: String },
}
```

Sorry for the noise, though maybe someone with the same problem will find this issue later and find it helpful.

---

_Comment by @epage on 2023-06-22 01:46_

For future reference, you might find the [git example](https://docs.rs/clap/latest/clap/_derive/_cookbook/git_derive/index.html) useful as it show cases a lot of different subcommand features.

---

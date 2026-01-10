---
number: 3639
title: "Add the ability to add alternative names for subcommand variants with `#[clap]` attribute"
type: issue
state: closed
author: mitchmindtree
labels:
  - C-enhancement
assignees: []
created_at: 2022-04-18T08:51:37Z
updated_at: 2022-04-18T12:35:10Z
url: https://github.com/clap-rs/clap/issues/3639
synced_at: 2026-01-10T01:27:44Z
---

# Add the ability to add alternative names for subcommand variants with `#[clap]` attribute

---

_Issue opened by @mitchmindtree on 2022-04-18 08:51_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.9

### Describe your use case

It would be nice if we could add alternative names for subcommands.

E.g. a `Build(BuildCmd)` command for cargo might offer:

```
cargo build
```

It would be nice if we could easily add shorthand aliases for this command. E.g.

```
cargo b
```

I believe this is currently possible for options, but I didn't come across a solution for subcommands.

Currently, the only solution I'm aware of for aliasing subcommands is to add further variants, e.g. 

```rust
    Build(BuildCmd),
    B(BuildCmd),
```

however I think we run into duplicated help docs here.

### Describe the solution you'd like

Perhaps something along the lines of

```rust
    #[clap(alias = "b")]
    Build
```

Or something like:

```rust
    #[clap(short)]
    Build
```
or
```rust
    #[clap(short = "b")]
```

By having a dedicated macro, we could generate dedicated documentation that describes the command is an alias or shorthand for the longer form.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @mitchmindtree on 2022-04-18 08:51_

---

_Referenced in [FuelLabs/sway#1285](../../FuelLabs/sway/pulls/1285.md) on 2022-04-18 08:52_

---

_Comment by @epage on 2022-04-18 12:10_

On the derive-reference, we explain under ["Raw attributes" that any `Command` builder method can be used as an attribute](https://github.com/clap-rs/clap/blob/v3.1.9/examples/derive_ref/README.md#command-attributes).

If you then go over to [`Command`](https://docs.rs/clap/latest/clap/type.Command.html), you'll see a link for subcommand settings, scroll down, and you'll see we have an [alias](https://docs.rs/clap/latest/clap/struct.App.html#method.alias) method.

Both of these combined mean you can do 
```rust
    #[clap(alias = "b")]
    Build
```
though that doesn't show up in the `help` by default.  For that, you are looking for
```rust
    #[clap(visible_alias = "b")]
    Build
```

---

_Comment by @mitchmindtree on 2022-04-18 12:35_

Ahh thanks for the quick response, and apologies for the oversight! Hopefully this issue at leasts helps another clap wanderer in the future.

---

_Closed by @mitchmindtree on 2022-04-18 12:35_

---

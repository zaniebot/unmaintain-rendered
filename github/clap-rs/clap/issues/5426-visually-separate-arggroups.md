---
number: 5426
title: Visually Separate ArgGroups
type: issue
state: closed
author: Swivelgames
labels:
  - C-enhancement
assignees: []
created_at: 2024-03-25T22:04:48Z
updated_at: 2024-03-26T01:45:07Z
url: https://github.com/clap-rs/clap/issues/5426
synced_at: 2026-01-07T13:12:20-06:00
---

# Visually Separate ArgGroups

---

_Issue opened by @Swivelgames on 2024-03-25 22:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.7

### Describe your use case

ArgGroups are very useful for developers, but they don't always show up sequentially in the help text, which can make it difficult to decipher between different options.

For instance, all `+` are part of the "Initializer" `ArgGroup`, and the `-` are this specific subcommand's options. The rest are `global = true` options:

```diff
  $ dotctl init --help
  Initializes a new directory in dotctl for a new machine

  Usage: dotctl machine init [OPTIONS] <--template <TEMPLATE>|--copy-from <COPY_FROM>|--bare>

  Options:
        --debug                  Prints extra information
+   -t, --template <TEMPLATE>    Use a template as the base for this machine
+   -c, --copy-from <COPY_FROM>  Use an existing machine config as the base for this machine
    -d, --dry-run                Prints actions, but doesn't perform them
+       --bare                   Initialize an empty machine
        --mock                   
-       --home <HOME>            Use a different destination than $HOME
-   -m, --machine <MACHINE>      Specify a different hostname
-       --force                  
    -h, --help                   Print help
    -V, --version                Print version
```

### Describe the solution you'd like

It would be great if we could visually separate options and arguments from the rest of the list if they're grouped, to make it easier to read for users:

Again, all `+` are part of the "Initializer" `ArgGroup`, and the `-` are this specific subcommand's options. The rest are `global = true` options:


```diff
  $ dotctl init --help
  Initializes a new directory in dotctl for a new machine

  Usage: dotctl machine init [OPTIONS] <--template <TEMPLATE>|--copy-from <COPY_FROM>|--bare>

  Init Options:
-       --home <HOME>            Use a different destination than $HOME
-   -m, --machine <MACHINE>      Specify a different hostname
-       --force                  

  Initializer Options:
+   -t, --template <TEMPLATE>    Use a template as the base for this machine
+   -c, --copy-from <COPY_FROM>  Use an existing machine config as the base for this machine
+       --bare                   Initialize an empty machine config

  Global Options:
        --debug                  Prints extra information
    -d, --dry-run                Prints actions, but doesn't perform them
        --mock
    -h, --help                   Print help
    -V, --version                Print version
```

I'm imagining this implemented with something like `#[group(distinguish = true)]`:

```rust
#[derive(ClapArgs, Clone)]
#[group(required = true, multiple = false, distinguish = true)]
pub struct Args {
	/// Use a template as the base for this machine
	#[arg(short, long, group = "Initializer")]
	template: Option<String>,

	/// Use an existing machine config as the base for this machine
	#[arg(short, long, group = "Initializer")]
	copy_from: Option<String>,

	/// Initialize an empty machine
	#[arg(long, action, group = "Initializer")]
	bare: bool,

	/// Specify a different hostname
	#[arg(short, long)]
	machine: Option<String>,

	#[arg(long, action)]
	force: bool,
}
```

---

_Label `C-enhancement` added by @Swivelgames on 2024-03-25 22:04_

---

_Comment by @epage on 2024-03-26 00:54_

Clap has this but its not expressed through groups.  As someone who mostly did CLIs in Python before Rust, this surprised me about clap but it turned out to be so much better.  Groups are a way of tagging arguments to define arbitrary relationships.  These can't boil down to simple things like "organize help by group".  Instead, clap has the concept of applying a `help_heading` to an argument.  See the [find example](https://docs.rs/clap/latest/clap/_derive/_cookbook/find/index.html) for what this looks like in code (builder API but concept carries over) and in the help output.

---

_Comment by @Swivelgames on 2024-03-26 01:45_

@epage Oh, awesome! I totally overlooked that. :sweat_smile: Thanks! Apologies for the frivolous issue :upside_down_face: 

---

_Closed by @Swivelgames on 2024-03-26 01:45_

---

```yaml
number: 6026
title: Add a way to put subcommands first, before required positional arguments
type: issue
state: closed
author: fck-gthb
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2025-06-08T07:18:30Z
updated_at: 2025-06-13T16:05:42Z
url: https://github.com/clap-rs/clap/issues/6026
synced_at: 2026-01-12T16:14:17Z
```

# Add a way to put subcommands first, before required positional arguments

---

_@fck-gthb_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.39

### Describe your use case

```
#[derive(Parser)]
#[command()]
struct Cli {
    #[command(subcommand)]
    command: Command,

    path: String,
}
```

`clap` insists on putting the subcommand last despite it being first in the struct:

`Usage: clap_test <PATH> <COMMAND>`

What I want instead is `clap_test <COMMAND> <PATH>`; `clap` somehow does it all backwards, which is counterintuitive.

Note that `path` is required, so I can't use `#[arg(global = true)]`. I also want to read `path` once before getting to the subcommands:

```rust
fn main() {
    let args = Cli::parse();
    read_file(args.path); // It's a common required argument.
    match args.command {
        Command::Simple { alpha } => todo!(),
        Command::Fancy { beta } => todo!(),
    }
}
```

Not like this:

```rust
#[derive(Subcommand)]
enum Command {
    Simple(SharedArgs),
    Fancy(SharedArgs),
}

#[derive(Args)]
struct SharedArgs {
    path: String,
}

fn main() {
    let args = Cli::parse();
    match args.command {
        Command::Simple(args) => {
            read_file(args.path); // repeating
        }
        Command::Fancy(args) => {
            read_file(args.path);  // repeating
        }
    }
}
```

I could do this, but I lose the ability to use subcommand-specific arguments and parameters:

```rust
#[derive(Parser)]
#[command()]
struct Cli {
    command: Command,

    path: String,
}


#[derive(Clone, clap::ValueEnum)]
enum Command {
    Simple, // ValueEnum only supports unit variants
    Fancy, // ValueEnum only supports unit variants
}

#[derive(Args, Clone)]
struct SharedArgs {
    path: String,
}

fn main() {
    let args = Cli::parse();
    match args.command {
        Command::Simple => {}
        Command::Fancy => {}
    }
}
```

### Describe the solution you'd like

A possible solution: make the order matter.

The subcommand first, then `path`:

```rust
#[derive(Parser)]
#[command()]
struct Cli {
    #[command(subcommand)]
    command: Command,

    path: String,
}
```

`path` first:

```rust
#[derive(Parser)]
#[command()]
struct Cli {
    path: String,

    #[command(subcommand)]
    command: Command,
}
```


### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @fck-gthb on 2025-06-08 07:18_

---

_Label `A-parsing` added by @epage on 2025-06-09 14:14_

---

_Comment by @epage on 2025-06-09 14:22_

> `clap` insists on putting the subcommand last despite it being first in the struct:
> 
> `Usage: clap_test <PATH> <COMMAND>`
> 
> What I want instead is `clap_test <COMMAND> <PATH>`; `clap` somehow does it all backwards, which is counterintuitive.

CLIs are generally context-sensitive parsers.  What you are currently parsing is dependent on the context of what came before.  Especially with subcommands, everything after a subcommand is exclusively parsed as part of that subcommand.  Besides the complexity and user confusion over allowing something like this, it can degrade the quality of error messages.  The closest we are looking to change this is with #2222 which we still need to be very careful with.



> Note that `path` is required, so I can't use `#[arg(global = true)]`.

The issue for this is #5977.



> I also want to read `path` once before getting to the subcommands:

While I can understand the convenience of this, implementing this comes at a cost build time and binary size cost for everyone to have this behavior.  This would also either be a breaking change, which would have to wait for 5.0, or a new flag which also balloons the API.  We've found the larger the API, the less likely people will use whats in the API and will instead hand roll things, so by adding something to the API we lower the value of everything in the API, including the item being added.

I've also found that in practice, a lot of people put things as global arguments for code reuse or because they assume something is truly global, to later find it isn't and regret it.  I suggest extreme caution in even using `global`.  I think `cargo --frozen` / `cargo --locked`, and `cargo --offline` are good examples of this.  In fact, no non-modal global in `cargo` makes sense in the presence of external subcommands which confuses users.

Overall, this feels like a fairly specialized case that has a lot of negatives that I'm going to go ahead and close this.  If there is a reason we should re-evaluate, let us know!

---

_Closed by @epage on 2025-06-09 14:22_

---

_Comment by @fck-gthb on 2025-06-13 16:05_

So the way to go is to implement my own argument parser. Okay.

---

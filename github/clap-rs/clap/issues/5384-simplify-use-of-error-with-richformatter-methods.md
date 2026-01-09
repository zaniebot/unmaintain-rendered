---
number: 5384
title: "Simplify use of Error with RichFormatter (methods `print` and `exit`)"
type: issue
state: closed
author: vrurg
labels:
  - C-enhancement
assignees: []
created_at: 2024-03-02T03:22:51Z
updated_at: 2024-03-12T22:57:17Z
url: https://github.com/clap-rs/clap/issues/5384
synced_at: 2026-01-07T13:12:20-06:00
---

# Simplify use of Error with RichFormatter (methods `print` and `exit`)

---

_Issue opened by @vrurg on 2024-03-02 03:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5

### Describe your use case

I'm implementing a build helper tool for my local project. The tool needs to know the base directory of a crate which could either be provided for `--base` argument via a dedicated envvar, or guessed from a `CARGO_*` environment variable(s). When it falls through to the guessing fallback and still has no success I'd like the tool to provide a detailed message with a self-explaining error message and tips. Looks like a job for the `RichFormatter`, but it turns out that method `print` doesn't respect error's formatter. This also results in `exit` method printing simplified error.

So far, I came down to the following sample to achieve my goal (the code is for use with `rust-script`):

```rust
//! ```cargo
//! [dependencies]
//! clap = { version = "4.5", features = ["derive", "env", "wrap_help", "error-context"] }
//! ```

use clap::{
    builder::StyledStr,
    error::{ContextKind, ContextValue, ErrorFormatter, ErrorKind, RichFormatter},
    CommandFactory, Parser,
};

#[derive(Parser)]
#[command(name = "my-builder", version, about)]
struct Cli {
    /// Project root
    #[arg(long, short, env = "MY_BASE", default_missing_value = "none")]
    base: Option<String>,
}

fn validate_cli(cli: &Cli) {
    if cli.base.is_none() {
        use std::fmt::Write as _;
        let mut cmd = Cli::command();
        let mut err = cmd
            .error(ErrorKind::MissingRequiredArgument, "Can't determine base directory")
            .apply::<RichFormatter>();
        let mut suggestion = StyledStr::new();
        let _ = write!(suggestion, "try running under `cargo make`");
        err.insert(ContextKind::Suggested, ContextValue::StyledStrs(vec![suggestion]));
        let s = RichFormatter::format_error(&err);
        eprintln!("{}", s.ansi());
        eprintln!("------");
        err.exit();
    }
}

fn main() {
    let cli = Cli::parse();
    validate_cli(&cli);
}
```

Here what it comes up with:

<img width="768" alt="image" src="https://github.com/clap-rs/clap/assets/1586992/c4f235f2-cb2f-47a9-a682-d97318b6cab3">

Unfortunately, despite of having the tip in the output, my custom message and `Usage:` are both lost. So far, I can compensate for the message by reporting it manually and `Usage:` isn't that critical (though pretty). But what is worse is that `print` does take into account if terminal status of stdout, whereas `ansi` doesn't:

<img width="747" alt="image" src="https://github.com/clap-rs/clap/assets/1586992/dace66bf-8405-4b86-bf2c-536725fb49b6">

Another annoyance is the complexity of creating a suggestion. It's not even nowhere in the docs to be found about what `ContextValue` variant is required for each of `ContextKind`.

### Describe the solution you'd like

It would be best if the `validate_cli` function could be reduced to:

```rust
fn validate_cli(cli: &Cli) {
    if cli.base.is_none() {
        let mut cmd = Cli::command();
        let mut err = cmd
            .error(ErrorKind::MissingRequiredArgument, "Can't determine base directory")
            .apply::<RichFormatter>();
        err.insert(ContextKind::Suggested, ContextValue::String("try running under `cargo make`"));
        err.exit();
    }
}
```


### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @vrurg on 2024-03-02 03:22_

---

_Comment by @epage on 2024-03-03 03:07_

The core of the confusion is that `Command::error` is meant for pre-formatted errors and the error formatter is not applied.  At this moment, I do not remember the design constraints that led to all of this.

The Context and Formatter APis are generally for more advanced workflows (really more of an escape hatch) and so not as much attention as been put towards documenting and polish.

Some options include:
- Write a styled string containing the information you want with `Command::error`
- Explicitly format the error and use `anstream` to print the message (this is what clap uses under the hood)
- Just write your own message, looking like clap

Some quick notes on the code sample:
- The `error-context` feature is a default feature, so it doesn't need to be called out
- `Error::apply` is mean for changing the formatter and so that is not needed as Rich is the default

---

_Comment by @vrurg on 2024-03-03 21:11_

> Some options include

What I love to do is to blend in into the style of the framework I'm using. Basically, after sorting out what I learned so far, here is what I really need.

When the base directory is not explicitly specified it could be guessed based on `cargo` (`cargo-make` in my case) environment. Only when this fails the error is to be printed. 

The task could be solved much easier with a kind of global hook on a command or subcommand. Something like `#[command(..., checkin = cli-checking)]` where `cli-checking` is `Fn(&Cli) -> Result<(), Error>`.



> Some quick notes on the code sample

Those are actually artifacts of me experimenting. Eventually, `anstream` is the last piece of puzzle to finish my implementation. It's somewhat more compact comparing to to the test code here. But it's only thanks to three additional traits implementing conversion from `ContextValue`, `Display`, and `Iterator` into `ContextValue::StyledStrs`; and forth trait to add a method onto `clap::error::Error` to use these when adding a suggestion. I love to have simple APIs, even if they come at cost of complex-ish internals. ;)

---

_Comment by @epage on 2024-03-12 21:00_

> The task could be solved much easier with a kind of global hook on a command or subcommand. Something like #[command(..., checkin = cli-checking)] where cli-checking is Fn(&Cli) -> Result<(), Error>.

More extensible validation is being discussed in #3476.  In general, I am against baking in specialized validation for every need.  Pushing too much validation onto clap would make clap both harder to read and make most clients harder to read.  In general, more I encourage more advanced configuration to be managed manually within the application.

---

_Comment by @vrurg on 2024-03-12 22:57_

> In general, more I encourage more advanced configuration to be managed manually within the application.

That's what I ended up with anyway. More friendly interface for producing errors would be nice to have, but there is no reason to insist on anything. So, I think it worth closing the issue.

---

_Closed by @vrurg on 2024-03-12 22:57_

---

---
number: 6109
title: "Add default missing values of `Arg` to help message"
type: issue
state: open
author: tvercruyssen
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2025-08-23T23:28:41Z
updated_at: 2025-10-24T14:36:05Z
url: https://github.com/clap-rs/clap/issues/6109
synced_at: 2026-01-10T01:28:22Z
---

# Add default missing values of `Arg` to help message

---

_Issue opened by @tvercruyssen on 2025-08-23 23:28_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Command line options with a `default_missing_value` can be used as flags and then take on a specified default value. This behavior is quite useful. However, the user discoverability of it seems to be non-existent? As in, based on the help output a user couldn't know whether an option has a `default_missing_value`. E.g.:
```console
Usage: clap-6109 [OPTIONS]

Options:
      --color [<COLOR>]  [default: auto] [possible values: auto, always, never]
  -h, --help             Print help
  -V, --version          Print version
```

<details>
<summary>Source code</summary>

```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive"] }
---

use clap::Parser;

#[derive(Parser, Debug)]
#[command(version, about, long_about = None)]
struct Cli {
    #[arg(
        long,
        value_parser = ["auto", "always", "never"],
        default_value = "auto",
        default_missing_value = "always",
        num_args = 0..=1,
    )]
    color: String,
}

fn main() {
    let cli = Cli::parse();
    println!("{cli:#?}");
}

```

</details>

### Describe the solution you'd like

Add default missing values to the generated help message. If included/displayed the same way as default values, it would look like:

```console
Usage: clap-6109 [OPTIONS]

Options:
      --color [<COLOR>]  [default: auto] [default missing value: always] [possible values: auto, always, never]
  -h, --help             Print help
  -V, --version          Print version
```
Potentially, this should be behind a flag and not the default behavior to include these. Moreover, I have no preference on the format and welcome discussion about it. It seems sensible enough to adopt the already used format for this, hence the proposal.

### Alternatives, if applicable

The only way I see a user discovering this behavior is through the help message

### Additional Context

This is my first feature request, please point out any mistakes I made 🥺 .

---

_Label `C-enhancement` added by @tvercruyssen on 2025-08-23 23:28_

---

_Referenced in [clap-rs/clap#6110](../../clap-rs/clap/pulls/6110.md) on 2025-08-23 23:41_

---

_Label `A-help` added by @epage on 2025-08-25 14:36_

---

_Label `S-waiting-on-decision` added by @epage on 2025-08-25 14:37_

---

_Comment by @epage on 2025-08-25 14:41_

```console
Usage: n --ex1 <EXAMPLE> --ex2 <EXAMPLE2>

Options:
      --ex1 <EXAMPLE>   Ex1
      --ex2 <EXAMPLE2>  Ex2
  -h, --help            Print help
  -V, --version         Print version
```
```rust
use clap::Parser;

#[derive(Parser)]
#[command(version, about, long_about = None)]
struct Cli {
    /// Ex1
    #[arg(long = "ex1")]
    example: String,

    /// Ex2
    #[arg(long = "ex2", default_missing_value = "some_value")]
    example2: String,
}

fn main() {
    Cli::parse();
}
```

Note that there is a bug in this program, `--ex2` always requires a value, despite `default_missing_value` being set.

I'll update the included examples to fix the bug and to be more real-world

---

_Comment by @epage on 2025-08-25 14:50_

The `[]` context / specs have never really sat right with me and the more we add, the worse I feel the overall experience is.

Take the updated example where we have three pieces of context and how it overwhelms things.
```console
$ ./clap-6109.rs --help
Usage: clap-6109 [OPTIONS]

Options:
      --color [<COLOR>]  [default: auto] [default missing value: always] [possible values: auto, always, never]
  -h, --help             Print help
  -V, --version          Print version

$ ./clap-6109.rs
Cli {
    color: "auto",
}

$ ./clap-6109.rs --color
Cli {
    color: "always",
}

$ ./clap-6109.rs --color never
Cli {
    color: "never",
}
```

#5156 might see us move `possible values` out.

In the case of `--color`, I feel like the `default_missing_value`s meaning is clear.  I suspect that should be the case for all uses of it or else its crossing over into cute-but-not-helpful territory.  If my suspicion is correct, then adding a `[default missing value]` context becomes redundant.

Could you help me understand what it is that you see where showing this information would be helpful and outweigh the costs?

---

_Comment by @tvercruyssen on 2025-08-26 18:35_

The real-world use case is a toy app that removes (neo)vim undofiles:
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive"] }
---

use clap::Parser;

#[derive(Parser, Debug)]
#[command(version, about, long_about = None)]
pub struct Cli {
    // <SNIP/>

    /// Purge files with an older modified date
    #[arg(short = 'l', long, default_missing_value = "90d", num_args = 0..=1,)]
    modified_date_limit: Option<String>, // Option<humantime::Duration>
}

fn main() {
    let cli = Cli::parse();
    println!("{cli:#?}");
}
``` 
Where the `default_missing_value` isn't necessarily obvious, but it did seem useful that the user could use the 'recommended' default threshold. And be easily informed about what it is. However, maybe I'm misusing the option, in which case by all means feel free to close this issue/associated MR.
If, however, you feel this is a valid use case, I admit that it is still rather niche and the default should potentially stay as is. I.e. no inclusion of `default_missing_value` in help messages.

Regarding the output, I don't really have an opinion on how it shows up, as long as the user understands it. But I will agree that it is currently a bit cluttery.

One option could be: 
```
$ ./clap-6109.rs --help
Usage: clap-6109 [OPTIONS]

Options:
      --color [<COLOR>|'always']  [default: auto] [possible values: auto, always, never]
  -h, --help             Print help
  -V, --version          Print version
```
Tho I'm not sure whether that is unambiguous, i.e. whether the user understands `<COLOR>` is their input and `always` is the default if not given. But it does reduce clutter. And I think it illustrates the idea of inlining/using symbols/context to indicate the meaning rather than the full term `default_missing_value`. 

---

_Comment by @epage on 2025-08-26 18:49_

> The real-world use case is a toy app that removes (neo)vim undofiles:

Thanks for that example!  I'll have to think on this

> One option could be:
>
> Tho I'm not sure whether that is unambiguous

imo it is.  Its saying "if a value is present, it can be one of `COLOR` or the literal string `always`".

---

_Comment by @tvercruyssen on 2025-08-26 19:11_

Hmm, perhaps
```console
$ ./clap-6109.rs --help
Usage: clap-6109 [OPTIONS]

Options:
      --color [<COLOR>=always]  [default: auto] [possible values: auto, always, never]
  -h, --help             Print help
  -V, --version          Print version
```
I.e. `=` instead of `|`. Tho this seems more ambiguous in that it may indicate that no user input can be given...

---

_Comment by @carlocorradini on 2025-10-24 14:36_

Hey, this is super useful 😍
Any update? 🙌

---

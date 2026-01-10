---
number: 6013
title: "Ability to control `--help` coloring using CLI args (e.g. `--color`) with derive syntax"
type: issue
state: open
author: ivan-aksamentov
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2025-05-23T22:19:04Z
updated_at: 2025-05-30T23:30:05Z
url: https://github.com/clap-rs/clap/issues/6013
synced_at: 2026-01-10T01:28:20Z
---

# Ability to control `--help` coloring using CLI args (e.g. `--color`) with derive syntax

---

_Issue opened by @ivan-aksamentov on 2025-05-23 22:19_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.2

### Describe your use case

While clap can handle TTY and non-TTY outputs as well the environment variables co control color well, many applications also have additional CLI args for that, typically  ``

Demonstrate --color has no effect on --help


```rust
use clap::{ArgEnum, Parser};

#[derive(Parser)]
struct Cli {
  #[clap(long, global = true, arg_enum)]
  color: Option<ColorOption>,
}

#[derive(Copy, Clone, ArgEnum)]
enum ColorOption {
  Auto,
  Always,
  Never,
}

fn main() {
  let args = Cli::parse(); // No way to account for color mode in `--help`
}

```

When running 

```
cli --help --color=never
```

it prints color output, because clap does not know about my custom `--color` arg.



### Describe the solution you'd like

It seems parsing and printing help should not be a monolithic function call. I'd like parsing and help (without re-implementing manually) to be distinct operations, such that I could insert color logic between them:

```rust
# Hypothetical code
let args = CLI::parse();
let is_colored = calculate_is_colored_based_on(&args, &env);
let style = if is_colored { Some(my_style) } else { None };
args.maybe_print_help(style); # need to handle subcommands too
```

I would also enjoy built-in `--color=always|never|auto` and `--no-color` (shortcut flag for `--color=never`), similarly to built-in `--help`. Clap already detects TTY and env vars, so half of the work is already done. Plus, the color args are virtually ubiquitous. Though not sure how the result would be exposed for usage (`--help` does not expose any result, because it does not have any).

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @ivan-aksamentov on 2025-05-23 22:19_

---

_Label `A-help` added by @epage on 2025-05-26 17:35_

---

_Label `S-waiting-on-design` added by @epage on 2025-05-26 17:35_

---

_Comment by @epage on 2025-05-26 17:43_

Cargo's issue for this is rust-lang/cargo#9012

You can disable the built-in help and provide your own help flag (of type `bool`) and render the help directly.  There are limitations and annoyances with this.

A more out-there approach could be
1. Parse using `try_parse`
2. If the error is "help", get a command-factory and re-parse with a custom help flag
3. Check the `--color` flag and then decide how to render the error returned by `try_parse`

Overall, the some challenges for a more built-in way are:
- Help is acted on immediately so we don't see any later flags
- Help bypasses any other forms of validation
- Help uses information from the parser for being rendered

Especially when it comes to the derive, the first two can mean that the derive can't populate the struct, preventing you from acting on it.

The last makes it more difficult to have this be a separate operation.  There is also the convenience factor for users in having them combined.  Splitting them might come at the risk of specialized APIs that might go counter to our efforts for shrinking and focusing the API for easier discoverability and better binary size.

---

_Comment by @ivan-aksamentov on 2025-05-26 19:59_

> You can disable the built-in help and provide your own help flag (of type bool) and render the help directly. There are limitations and annoyances with this.

In my particular case, I have many subcommands, and I am not sure how to handle help for all of them. 

And I tried 2-step parsing, but this is where I got stuck:

> Help is acted on immediately so we don't see any later flags

It seems if `--help` is present, `--color` is not being parsed.

---

> There is also the convenience factor for users in having them combined.

Just to clarify, I am not proposing removing the combined function, but rather to expose some hooks to do things in userspace

---

> might go counter to our efforts for shrinking and focusing the API 

Fair enough

---

Thanks for detailed reply!


---

_Referenced in [nextstrain/nextclade#1629](../../nextstrain/nextclade/issues/1629.md) on 2025-05-27 06:47_

---

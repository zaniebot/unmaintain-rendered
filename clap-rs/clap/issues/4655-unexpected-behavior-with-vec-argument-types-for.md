```yaml
number: 4655
title: "Unexpected behavior with `Vec<_>` argument types for positionals and options"
type: issue
state: closed
author: lucatrv
labels:
  - C-bug
assignees: []
created_at: 2023-01-22T15:14:51Z
updated_at: 2023-04-24T00:56:03Z
url: https://github.com/clap-rs/clap/issues/4655
synced_at: 2026-01-12T16:14:16Z
```

# Unexpected behavior with `Vec<_>` argument types for positionals and options

---

_@lucatrv_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.66.1

### Clap Version

4.1.1

### Minimal reproducible code

Positional:
```rust
#[derive(Debug, Parser)]
struct Args {
    positional: Vec<String>,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

Option:
```rust
#[derive(Debug, Parser)]
struct Args {
    #[arg(long)]
    option: Vec<String>,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

Option with fix:
```rust
#[derive(Debug, Parser)]
struct Args {
    #[arg(long, num_args = 0..)]
    option: Vec<String>,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

Positional and option:
```rust
#[derive(Debug, Parser)]
struct Args {
    positional: Vec<String>,
    #[arg(long)]
    option: Vec<String>,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

Positional and option with fix:
```rust
#[derive(Debug, Parser)]
struct Args {
    positional: Vec<String>,
    #[arg(long, num_args = 0..)]
    option: Vec<String>,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

### Steps to reproduce the bug with the above code

Positional:
`cargo run --  a b c`

Option:
`cargo run -- --option a b c`

Option with fix:
`cargo run --  --option a b c`

Positional and option:
`cargo run --  a b c --option d e f`

Positional and option with fix:
`cargo run --  a b c --option d e f`

### Actual Behaviour

Positional:
```
args = Args {
    positional: [
        "a",
        "b",
        "c",
    ],
}
```

Option:
```
error: unexpected argument 'b' found
```

Option with fix:
```
args = Args {
    option: [
        "a",
        "b",
        "c",
    ],
}
```

Positional and option:
```
args = Args {
    positional: [
        "a",
        "b",
        "c",
        "e",
        "f",
    ],
    option: [
        "d",
    ],
}
```

Positional and option with fix:
```
args = Args {
    positional: [
        "a",
        "b",
        "c",
    ],
    option: [
        "d",
        "e",
        "f",
    ],
}
```

### Expected Behaviour

Positional:
```
args = Args {
    positional: [
        "a",
        "b",
        "c",
    ],
}
```

Option:
```
args = Args {
    option: [
        "a",
        "b",
        "c",
    ],
}
```

Option with fix:
```
args = Args {
    option: [
        "a",
        "b",
        "c",
    ],
}
```

Positional and option:
```
args = Args {
    positional: [
        "a",
        "b",
        "c",
    ],
    option: [
        "d",
        "e",
        "f",
    ],
}
```

Positional and option with fix:
```
args = Args {
    positional: [
        "a",
        "b",
        "c",
    ],
    option: [
        "d",
        "e",
        "f",
    ],
}
```

### Additional Context

As shown by the sample code above, when a positional argument is defined with `Vec<_>` type, `num_args(0..)` is implied as expected. However when an option argument is defined with the same type, `num_args(0..)` is not implied, so it needs to be added manually to achieve the same behavior (see code above "with fix"). This leads to unexpected behavior in particular when using both positional and option arguments. IMHO `num_args(0..)` should be implied for both positional and option arguments.

### Debug Output

_No response_

---

_Label `C-bug` added by @lucatrv on 2023-01-22 15:14_

---

_Comment by @epage on 2023-01-23 14:45_

So if I'm understanding correctly, you are wanting `Vec` for options to accept multiple values per flag?

That was the old structopt behavior and we intentionally switched in #2993 as
- `ArgAction::Append` is the more commonly expected command-line behavior, next followed by also setting `value_delimiter`
- `num_args(0..)` is not without problems [which we warn about in the docs](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.num_args)

---

_Comment by @lucatrv on 2023-01-24 13:27_

Thanks for your answer, your [reference](https://github.com/clap-rs/clap/pull/2993) helped me better understand. However what I find confusing is that `Vec<_>` leads to different default behaviors for positional and option arguments. This [table](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types) seems valid only for option arguments, for which `action(ArgAction::Append)` is implied, while it does not seem valid for positional arguments, for which `num_args(0..)` is actually implied.
In my opinion the same behavior for positional or option arguments (both with `num_args(0..)` implied) would have been the most straightforward solution, but I now understand why it was decided to differentiate. If this is wanted by design, and I'm in agreement after all, IMHO this different behavior should be better documented.
For instance, a new column could be added to this [table](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types) to explain what is implied for positional arguments. Moreover it would help to add a note explaining how to change the behavior of option arguments from the default one using multiple occurrences (`action(ArgAction::Append)`) to use value delimiter (`value_delimiter = ','`), or to achieve the same default behavior of positional arguments (`num_args(0..)`). For the latter case, a warning could explained why it is not considered the default behavior for option arguments and why it is not advised (referring to [this one](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.num_args)).
IMHO this would clarify much better this matter to new users.


---

_Comment by @epage on 2023-01-31 22:08_

> For instance, a new column could be added to this [table](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types) to explain what is implied for positional arguments.

The reason I didn't go into more nuance in the table is that `Append` implies the desired behavior for both.  The more we complicate the table, the harder it will be for people to use.

>  Moreover it would help to add a note explaining how to change the behavior of option arguments from the default one using multiple occurrences (action(ArgAction::Append)) to use value delimiter (value_delimiter = ','), or to achieve the same default behavior of positional arguments (num_args(0..)

The derive reference is exclusive to what is unique in the derive.  See https://github.com/clap-rs/clap/discussions/4090 for more details and further discussion on how to handle that problem.

As for what cases to cover in other forms of documentation (cookbook, tutorials), the challenge is finding the right subset to cover and rely on the builder reference for the rest or else it becomes unmanageable.

As I feel everything from this is expected behavior, I'm closing this out.  We can continue the discussion if you want which can always lead to either this specific issue being re-opened or new issues being created.

---

_Closed by @epage on 2023-01-31 22:08_

---

_Comment by @Arcitec on 2023-04-24 00:56_

@epage I want to thank both of you for this discussion. I stumbled on it as I was looking for a way to collect all positional arguments regardless of where they appear in the argument list.

I agree that named arguments should default to exactly 1 value, the way CLAP works by default now. I'd be very confused if a sudden `a b --option something d e` swallowed the `d e` afterwards by default!

I think CLAP works perfectly already. The "positional and option" (without "fix") in the list above makes the most sense and I am glad to see it's the default behavior. Thank you @lucatrv for the code demo that showed how to set that up.

Just for reference, that's the following code:

```rust
#[derive(Debug, Parser)]
struct Args {
    positional: Vec<String>,
    #[arg(long)]
    option: Vec<String>,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

Command line: `a b c --option d e f`

Result:

```
args = Args {
    positional: [
        "a",
        "b",
        "c",
        "e",
        "f",
    ],
    option: [
        "d",
    ],
}
```

It's perfect. But I am also glad to know that it's possible to override the capturing via the `num_args` parameter. :)

---

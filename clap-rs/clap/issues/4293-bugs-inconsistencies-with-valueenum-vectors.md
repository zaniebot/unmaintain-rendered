```yaml
number: 4293
title: "Bugs & Inconsistencies with ValueEnum Vectors"
type: issue
state: closed
author: ttytm
labels:
  - C-bug
assignees: []
created_at: 2022-09-29T21:11:56Z
updated_at: 2022-09-29T21:23:27Z
url: https://github.com/clap-rs/clap/issues/4293
synced_at: 2026-01-12T16:14:15Z
```

# Bugs & Inconsistencies with ValueEnum Vectors

---

_@ttytm_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0

### Clap Version

v4 | v3

### Minimal reproducible code

```rust
use clap::{Parser, ValueEnum};

#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Cli {
	/// What mode to run the program in
	#[arg(value_enum, long, short)]
	format: Vec<Format>,
}

#[derive(Debug, Copy, Clone, PartialEq, Eq, PartialOrd, Ord, ValueEnum)]
enum Format {
	Txt,
	Md,
	Html,
}

fn main() {
	let cli = Cli::parse();
	println!("{:?}", cli);
}

```


### Steps to reproduce the bug with the above code

Pass multiple enum values to an option arg (when long/short attributes are specified)
`cargo run -- -f txt,html` or `cargo run -- -f txt html`

---

To give a short context for the following sections:
When used as argument `#[arg(value_enum)]` _(without long/short attributes)_ it works.
It has to be passed separated with spaces. Comma separation won't work!
`txt html`

### Actual Behaviour

Depending on the structure of the program, passing multiple values either will throw an error or only the first value will be used (when passed separated with spaces).

In case of the code example above, either way of passing multiple values will produce an error when the arg has `long || short` attributes specified.


### Expected Behaviour

Passing multiple enum values to an option works.

### Additional Context

Using a `Vec<String>` works of course. 

It works with good ol `structopt`.  
Here values need to be passed comma separated `txt,html`.

In the end I'm hoping that I just missed a thing in the docs and doing something wrong, as this seems like a rather basic funcitonality and it's probably possible to reenable it for argument options?

### Debug Output

_No response_

---

_Label `C-bug` added by @ttytm on 2022-09-29 21:11_

---

_Comment by @epage on 2022-09-29 21:23_

I recommend checking out changelog to work through upgrades.  Since you are upgrading from structopt, in particular the [v3 breaking changes](https://github.com/clap-rs/clap/blob/v3-master/CHANGELOG.md#breaking-changes).  We also recommend upgrading to v3 before upgrading to v4.  All of our documentation is focused on that upgrade path.

> `cargo run -- -f txt html`

In clap 3, the meaning of `Vec` changed in the derive from `multiple_values(true).multiple_occurrences(true)` to `multiple_occurrences(true)`.

To get the structopt behavior for that in clap 3, add `#[clap(multiple_values(true))]`.

This was covered under "From structopt 0.3.25" as

> Vec<_> and Option<Vec<_>> have changed from multiple to multiple_occurrences

I will say that multiple values can cause a lot of problems.  We warn about some [in the docs](https://docs.rs/clap/3.2.22/clap/builder/struct.Arg.html#method.multiple_values)

> cargo run -- -f txt,html

I think this breaking change was missed in the changelog.  We made this opt-in.  You just need to add `#[clap(use_value_delimiters = true)]`.


As both parts of this are expected changes, I'm going to close this out.  If there is something I missed, let me know.  

---

_Closed by @epage on 2022-09-29 21:23_

---

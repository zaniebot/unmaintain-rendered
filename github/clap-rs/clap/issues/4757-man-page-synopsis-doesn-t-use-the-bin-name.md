---
number: 4757
title: "Man page synopsis doesn't use the bin_name"
type: issue
state: open
author: arusahni
labels:
  - C-bug
  - A-man
assignees: []
created_at: 2023-03-11T21:37:35Z
updated_at: 2023-03-20T14:27:33Z
url: https://github.com/clap-rs/clap/issues/4757
synced_at: 2026-01-07T13:12:20-06:00
---

# Man page synopsis doesn't use the bin_name

---

_Issue opened by @arusahni on 2023-03-11 21:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.67.1

### Clap Version

4.1.8

### Minimal reproducible code

```rust
use clap::{arg, CommandFactory, Parser};
use clap::CommandFactory;
use clap_mangen::Man;

#[derive(Debug, Parser)]
#[command(
    bin_name = "myapp",
)]
struct Cli {
   #[arg(long)]
    list: bool
}

fn main() {
    let cmd = Cli::comand();
    let man = Man::new(cmd);
    let mut buffer: Vec<u8> = Default::default();
    man.render(&mut buffer)?;
    Ok(())
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The generated manpage has a synopsis section that uses the Crate name for the sample invocation:

```text
SYNOPSIS
       appname [--list]
```

### Expected Behaviour

The generated manpage should have synopsis section that uses the provided `bin_name` for the sample invocation:

```text
SYNOPSIS
       myapp [--list]
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @arusahni on 2023-03-11 21:37_

---

_Referenced in [clap-rs/clap#4758](../../clap-rs/clap/pulls/4758.md) on 2023-03-11 21:39_

---

_Renamed from "Man page synopsis" to "Man page synopsis doesn't use the bin_name" by @arusahni on 2023-03-11 23:23_

---

_Label `A-man` added by @epage on 2023-03-13 17:20_

---

_Comment by @epage on 2023-03-13 17:21_

Is there a reason you are setting `bin_name` and not `name` or `display_name`?

---

_Comment by @arusahni on 2023-03-19 01:47_

Yes: I'm writing a git extension. By convention, git extensions are discovered by naming the binary `git-<extension name>`, but invoked as a git subcommand (`git <extension name>`). Ultimately, I'd like to reflect this nuance on the manpage, with the displayed name being `git-<extension name>`, but the synopsis showing `git <extension name>`.

`name` doesn't suit my need here because I don't want the subcommand variant displayed throughout the manpage, just places where an invocation example is provided. `display_name` does not currently impact the synopsis (my PR updates it to be considered, but with lower precedence than the `bin_name`).

---

_Comment by @epage on 2023-03-20 14:27_

Thanks for the explanation!

I think its reasonable then for this to go forward if we can find a way to make this work out

---

---
number: 4812
title: Value terminator is not included anywhere in help message
type: issue
state: open
author: epbuennig
labels:
  - C-enhancement
  - A-help
  - E-easy
assignees: []
created_at: 2023-03-30T16:22:16Z
updated_at: 2024-11-14T16:22:02Z
url: https://github.com/clap-rs/clap/issues/4812
synced_at: 2026-01-10T01:28:01Z
---

# Value terminator is not included anywhere in help message

---

_Issue opened by @epbuennig on 2023-03-30 16:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

v4.2.0

### Describe your use case

The following rust program:
```rust
use clap::{Command, Arg};

Command::new("cmd")
    .arg(Arg::new("multiple_options").num_args(0..).value_terminator(":"))
    .arg(Arg::new("last").required(false))
    .get_matches_from(["cmd", "-h"]);
```

currently produces this output:
```
Usage: cmd [multiple_options]... [last]

Arguments:
  [multiple_options]...  
  [last]                 

Options:
  -h, --help  Print help
```
For a user, it is not clear that the options before `last` must be delimited with `:` so the last argument is not assumed to be part of `multiple_options`.

Ideally, it should be included in both the usage itself and the Argument list in some way, mentioning it in the help message for each argument which may use this style would become tedious.

### Describe the solution you'd like

I would expect the program to output something similar to this:
```
Usage: cmd [<multiple_options>... :] [last]

Arguments:
  [<multiple_options>... :]  
  [last]                 

Options:
  -h, --help  Print help
```
without any additional changes by default.

### Alternatives, if applicable

An alternative to this is specifying the usage directly, but this falls flat as soon as `a` is turned into an option, as it's representation in the option list cannot be changed.

It could also be explained in the help message of the argument itself, but I feel like the automatically generated usage strings should include this.

### Additional Context

The following program already produces includes the `--` token for arguments specified as `required(false)` and `last(true)` (although only in the usage), which is why I believe my feature request falls inline with the current design philosophy of clap.

```rust
use clap::{Command, Arg};

fn main() {
    Command::new("cmd")
        .arg(Arg::new("multiple_options").num_args(1..))
        .arg(Arg::new("last").required(false).last(true))
        .get_matches_from(["cmd", "-h"]);
}
```
```
Usage: cmd [multiple_options]... [-- <last>]

Arguments:
  [multiple_options]...  
  [last]                 

Options:
  -h, --help  Print help
```

---

_Label `C-enhancement` added by @epbuennig on 2023-03-30 16:22_

---

_Label `A-help` added by @epage on 2023-03-30 18:14_

---

_Label `E-easy` added by @epage on 2023-03-30 18:14_

---

_Comment by @epage on 2023-03-30 18:15_

I think your idea for having it in the argument list and in the usage like that works.  My only concern is how much this increases the binary size as this is a less commonly used feature and users can provide custom usages.  I'm open to at least an experimental implementation being made for us to evaluate.

---

_Comment by @epbuennig on 2023-03-30 21:35_

My main concern is that, if this is on one or multiple options instead of positional arguments, then trying to do it with a custom usage won't work quite so well. We can't currently overwrite the usage that is displayed in the Argument/Options list.

---

_Comment by @toddATavail on 2023-04-06 00:47_

I like this proposal, @epbuennig; I've used the underlying feature of Clap before, and it would definitely be nice if the help generator would show you the stop delimiter without the developer having to write a custom usage message. I'm generally for anything that quickly improves documentation quality and usability, and this feels like an easy win. I'm busy tonight, but I have some dedicated time tomorrow to work on this. I'll see what it does to binary size (seems like it should be a small positive delta?), and then we can see if your team likes it, @epage.

---

_Referenced in [clap-rs/clap#4829](../../clap-rs/clap/pulls/4829.md) on 2023-04-07 03:24_

---

_Comment by @toddATavail on 2023-04-07 04:40_

Okay, my PR is in. Ended up thrashing against the CI for a bit, so sorry if that generated any annoying transient noise. Looks like CI is happy now, and all tests (including the 3 that I added to test this small feature) are passing. I only checked the binary delta on my M2, and it was negligible. I've used Clap for years, and this was a fun way to give something (admittedly small) back on a quiet evening. Thanks for the great tool!

---

_Referenced in [clap-rs/clap#5392](../../clap-rs/clap/issues/5392.md) on 2024-03-11 14:30_

---

_Referenced in [clap-rs/clap#5817](../../clap-rs/clap/pulls/5817.md) on 2024-11-14 15:39_

---

_Comment by @Aethelflaed on 2024-11-14 16:22_

I'm keen on working on that, what's the desired solution in the end?

---

_Referenced in [clap-rs/clap#5995](../../clap-rs/clap/issues/5995.md) on 2025-05-09 14:30_

---

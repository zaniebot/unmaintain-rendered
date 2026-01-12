```yaml
number: 3367
title: When deriving Parser, multi-sentence doc comments have final period removed
type: issue
state: open
author: nyanpasu64
labels:
  - C-bug
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-01-29T04:57:45Z
updated_at: 2024-08-07T13:52:40Z
url: https://github.com/clap-rs/clap/issues/3367
synced_at: 2026-01-12T16:14:15Z
```

# When deriving Parser, multi-sentence doc comments have final period removed

---

_@nyanpasu64_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.0.13

### Minimal reproducible code

```rust
use std::path::PathBuf;

use clap::Parser;

#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    /// If omitted, installs a shortcut to itself to the "Send To" menu.
    /// If passed, installs a shortcut to the .exe to the start menu.
    exe: Option<PathBuf>,
}

fn main() {
    Cli::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- -h`

### Actual Behaviour

The doc comment consists of multiple sentences, and it's difficult to rewrite it in a single sentence without losing critical information. But the final period is removed, resulting in a jarring and ungrammatical result:

```
send-to-start 0.1.0

USAGE:
    send-to-start.exe [EXE]

ARGS:
    <EXE>    If omitted, installs a shortcut to itself to the "Send To" menu. If passed,
             installs a shortcut to the .exe to the start menu

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information
```

In other cases, I've had to spend effort rewriting my doc comments into a single summary sentence, followed by extra paragraphs of explanation. Perhaps it's better for users, perhaps not. As an author I wish it wasn't necessary to do so to avoid structopt breaking my help output.

Also as a user I dislike how `-h` prints short output and `--help` prints multi-paragraph output, even though the help output (`-h, --help`) implies that these 2 commands are interchangeable. As a result, I often don't even know that multi-paragraph help even exists in Rust apps. (Admittedly git has different -h and --help, but I don't especially like that either.)

### Expected Behaviour

Either don't strip trailing periods from doc comments by default, or add a struct-wide or per-field option to disable stripping the trailing period. Ideally you could detect multi-sentence comments and avoid stripping the trailing period, but this may be difficult to implement without false positives or negatives.

I'm not sure how to unify the behavior of -h and --help, or address the odd formatting of `--help` showing 1 sentence followed by paragraphs.

### Additional Context

Somewhat related discussions:

- https://github.com/TeXitoi/structopt/issues/341 found this behavior unexpected.
- https://github.com/clap-rs/clap/discussions/2247 dislikes the current long help format (1 sentence followed by a paragraph).

### Debug Output

_No response_

---

_Label `C-bug` added by @nyanpasu64 on 2022-01-29 04:57_

---

_Label `A-derive` added by @epage on 2022-01-31 22:06_

---

_Label `S-waiting-on-design` added by @epage on 2022-01-31 22:06_

---

_Comment by @epage on 2022-01-31 22:10_

Thanks for reporting this!

https://github.com/clap-rs/clap/issues/1015 already exists about `-h` / `--help`.

TeXitoi/structopt#341 has some interesting trade offs and a workaround (`verbatim_doc_comment`), on top of manually specifying it (`help` and `long_help` attributes).  In this case, that behavior falls flat.

In this specific case
```
    /// If omitted, installs a shortcut to itself to the "Send To" menu.
    /// If passed, installs a shortcut to the .exe to the start menu.
```
Two thoughts on approaches
- This command operates in two modes.  Sounds like one is user-facing for initialization and the other is used in the initialized state?  I would make the documentation focus on the case users will interact with it
- Grammatically, I would put emphasis on the case where an arg is present, something like `Installs a shortcut to the .exe in the start menu [default this program]`.

---

_Comment by @briankung on 2022-05-15 11:33_

Hi, just adding a link to the (presumably) upstream issue: https://github.com/TeXitoi/structopt/issues/341

---

_Comment by @obskyr on 2024-05-23 14:21_

This dovetails with #3198 in an unfortunate manner. The following are true:

* There is no way to put a period at the end of a help message without using `verbatim_doc_comment`, and
* There is no way to escape newlines when using `verbatim_doc_comment`.

Together, this means that there is **no way to end a help message with a period and split the doc comment across multiple lines** (without also introducing manual line breaks to the help message). You essentially have to choose between having the period stripped, introducing a bunch of manual newlines into the message, or fitting the doc comment on a single line.

---

_Referenced in [clap-rs/clap#3198](../../clap-rs/clap/issues/3198.md) on 2024-07-07 07:28_

---

_Comment by @zanieb on 2024-08-07 13:52_

Hi! Just want to share my use-case here — I'm wrapping Clap to generate reference documentation for my CLI and it'd be great to be able to retain periods in the website.

---

_Referenced in [astral-sh/uv#5869](../../astral-sh/uv/pulls/5869.md) on 2024-08-07 16:07_

---

```yaml
number: 5003
title: "mangen: verbatim_doc_comment has no effect"
type: issue
state: open
author: WhyNotHugo
labels:
  - C-enhancement
  - S-blocked
  - A-man
assignees: []
created_at: 2023-07-10T09:27:18Z
updated_at: 2025-12-10T19:59:16Z
url: https://github.com/clap-rs/clap/issues/5003
synced_at: 2026-01-12T16:14:16Z
```

# mangen: verbatim_doc_comment has no effect

---

_@WhyNotHugo_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.70.0 (90c541806 2023-05-31) (Alpine Linux 1.70.0-r4)

### Clap Version

version = "4.1.4"

### Minimal reproducible code

```rust
use std::io::Write;

use clap::{Parser, CommandFactory};

/// *shotman* takes a screenshot and shows it in a small floating thumbnail window.
///
/// Screenshots are saved immediately. The first of the following locations that is
/// defined is used (i.e.: if the environment variable is undefined, the next one on
/// the list is used).
///
/// - _$XDG_SCREENSHOTS_DIR/_
/// - _$XDG_PICTURES_DIR/screenshots/_
/// - _$HOME/screenshots/_
/// - The current directory.
///
/// *shotman* requires a compositor that supports *wlr_screencopy* and
/// *wlr_layer_shell*.
#[derive(Parser)]
#[clap(author, about, long_about, verbatim_doc_comment)]
pub struct Cli {
}

fn main() {
            let man = clap_mangen::Man::new(Cli::command());
            let mut buffer: Vec<u8> = Default::default();
            man.render_title(&mut buffer).expect("Render man page to buffer");
            man.render_name_section(&mut buffer).expect("Render man page to buffer");
            man.render_synopsis_section(&mut buffer).expect("Render man page to buffer");
            man.render_description_section (&mut buffer).expect("Render man page to buffer");

            let stdout = std::io::stdout();
            stdout
                .lock()
                .write_all(&buffer)
                .expect("Write buffer with rendered manpage to stdout");
}
```


### Steps to reproduce the bug with the above code

```sh
cargo add clap --features=derive
cargo run > tmp.man && man ./tmp.man
```

### Actual Behaviour

Text is mangled up due to pre-processing.

### Expected Behaviour

`verbatim_doc_comment` should minimise pre-processing when converting doc comments.

### Additional Context

The `--help` output of this example prints fine:

```
*shotman* takes a screenshot and shows it in a small floating thumbnail window.

Screenshots are saved immediately. The first of the following locations that is
defined is used (i.e.: if the environment variable is undefined, the next one on
the list is used).

- _$XDG_SCREENSHOTS_DIR/_
- _$XDG_PICTURES_DIR/screenshots/_
- _$HOME/screenshots/_
- The current directory.

*shotman* requires a compositor that supports *wlr_screencopy* and
*wlr_layer_shell*.
```

https://github.com/clap-rs/clap/pull/4444 seems remotely related, but not quite the same.

### Debug Output

```
[clap_builder::builder::command]Command::_build: name="ma"
[clap_builder::builder::command]Command::_propagate:ma
[clap_builder::builder::command]Command::_check_help_and_version:ma expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:ma
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
```

---

_Label `C-bug` added by @WhyNotHugo on 2023-07-10 09:27_

---

_Comment by @epage on 2023-07-10 14:27_

For easier reproduction with cargo-script (with `nargo` being a wrapper around `env -S cargo -Zscript`
`````rust
#!/usr/bin/env nargo
/*!
```cargo
[dependencies]
clap = { version = "4", features = ["derive"] }
clap_mangen = "0.2"
```
*/

use std::io::Write;

use clap::{CommandFactory, Parser};

/// *shotman* takes a screenshot and shows it in a small floating thumbnail window.
///
/// Screenshots are saved immediately. The first of the following locations that is
/// defined is used (i.e.: if the environment variable is undefined, the next one on
/// the list is used).
///
/// - _$XDG_SCREENSHOTS_DIR/_
/// - _$XDG_PICTURES_DIR/screenshots/_
/// - _$HOME/screenshots/_
/// - The current directory.
///
/// *shotman* requires a compositor that supports *wlr_screencopy* and
/// *wlr_layer_shell*.
#[derive(Parser)]
#[clap(author, about, long_about, verbatim_doc_comment)]
pub struct Cli {}

fn main() {
    Cli::parse();
    let man = clap_mangen::Man::new(Cli::command());
    let mut buffer: Vec<u8> = Default::default();
    man.render_title(&mut buffer)
        .expect("Render man page to buffer");
    man.render_name_section(&mut buffer)
        .expect("Render man page to buffer");
    man.render_synopsis_section(&mut buffer)
        .expect("Render man page to buffer");
    man.render_description_section(&mut buffer)
        .expect("Render man page to buffer");

    let stdout = std::io::stdout();
    stdout
        .lock()
        .write_all(&buffer)
        .expect("Write buffer with rendered manpage to stdout");
}
`````

---

_Comment by @epage on 2023-07-10 14:27_

The `--help` output
```
*shotman* takes a screenshot and shows it in a small floating thumbnail window.

Screenshots are saved immediately. The first of the following locations that is
defined is used (i.e.: if the environment variable is undefined, the next one on
the list is used).

- _$XDG_SCREENSHOTS_DIR/_
- _$XDG_PICTURES_DIR/screenshots/_
- _$HOME/screenshots/_
- The current directory.

*shotman* requires a compositor that supports *wlr_screencopy* and
*wlr_layer_shell*.

Usage: clap-5003

Options:
  -h, --help
          Print help (see a summary with '-h')
```

---

_Comment by @epage on 2023-07-10 14:28_

The ROFF
```roff
.ie \n(.g .ds Aq \(aq
.el .ds Aq '
.TH clap-5003 1  "clap-5003 "
.ie \n(.g .ds Aq \(aq
.el .ds Aq '
.SH NAME
clap\-5003 \- *shotman* takes a screenshot and shows it in a small floating thumbnail window.
.ie \n(.g .ds Aq \(aq
.el .ds Aq '
.SH SYNOPSIS
\fBclap\-5003\fR [\fB\-h\fR|\fB\-\-help\fR]
.ie \n(.g .ds Aq \(aq
.el .ds Aq '
.SH DESCRIPTION
*shotman* takes a screenshot and shows it in a small floating thumbnail window.
.PP
Screenshots are saved immediately. The first of the following locations that is
defined is used (i.e.: if the environment variable is undefined, the next one on
the list is used).
.PP
\- _$XDG_SCREENSHOTS_DIR/_
\- _$XDG_PICTURES_DIR/screenshots/_
\- _$HOME/screenshots/_
\- The current directory.
.PP
*shotman* requires a compositor that supports *wlr_screencopy* and
*wlr_layer_shell*.
```

---

_Comment by @epage on 2023-07-10 14:29_

An approx of what the man page looks like
```
  1 clap-5003(1)                                  General Commands Manual                                  clap-5003(>
  2
  3 NAME
  4        clap-5003 - *shotman* takes a screenshot and shows it in a small floating thumbnail window.
  5
  6 SYNOPSIS
  7        clap-5003 [-h|--help]
  8
  9 DESCRIPTION
 10        *shotman* takes a screenshot and shows it in a small floating thumbnail window.
 11
 12        Screenshots  are  saved  immediately. The first of the following locations that is defined is used (i.e.: >
 13        the environment variable is undefined, the next one on the list is used).
 14
 15        - _$XDG_SCREENSHOTS_DIR/_ - _$XDG_PICTURES_DIR/screenshots/_ - _$HOME/screenshots/_ - The current director>
 16
 17        *shotman* requires a compositor that supports *wlr_screencopy* and *wlr_layer_shell*.
```

---

_Comment by @epage on 2023-07-10 14:37_

Based on `--help`, I feel like `verbatim_doc_comment` is doing what it says.  The only question is about `clap_mangen`s behavior of only inserting paragraph markers on blank lines which is independent of any derive behavior.

The challenge is knowing hard line breaks from soft line breaks

---

_Comment by @WhyNotHugo on 2023-07-20 07:44_

I think that you're right; strictly speaking, `verbatim_doc_comment` is doing what it says.

I expected `verbatim_doc_comment` to apply for `clap_mangen` in the same way it applies to `--help`. It really doesn't make sense that it would apply to one and not the other since that means parsing the exact same text with different rules for both usages.

Do you have any thoughts here? Should `verbatim_doc_comment` also apply to `clap_mangen`, or should some new/different flag be used instead?

---

_Comment by @epage on 2023-07-20 14:07_

I lean towards keeping `verbatim_doc_comment` as a derive-only concern.

We could add a `clap_mangen` setting that is verbatim-ish.  This would most likely be blocked on the in-work plugin system being made public so we can add this config to an `Arg` without `clap` knowing about `clap_mangen`-specific behavior.

---

_Label `A-man` added by @epage on 2023-07-20 14:07_

---

_Label `C-bug` removed by @epage on 2023-07-20 14:07_

---

_Label `C-enhancement` added by @epage on 2023-07-20 14:07_

---

_Label `S-blocked` added by @epage on 2023-07-20 14:07_

---

_Comment by @pamburus on 2025-12-08 21:50_

I was wondering if it might be possible to implement this on the `Man` struct directly, rather than waiting for the plugin system to attach it to an `Arg`?

By placing the option on the `Man` builder in the `clap_mangen`, the configuration remains external to the `clap` crate, keeping it unaware of `mangen` behavior.

For example:
```rust
let man = clap_mangen::Man::new(cli::Opt::command()).verbatim(true);
```

The proposed change can be found [here](https://github.com/clap-rs/clap/compare/53a69215e481c237bff5599f93bbcc7bdc6218c5...pamburus:rust-clap:1926fb0f1dafbaf47721ddc107ca1a39863b2890?expand=1).

@epage 
Would an approach like this be acceptable?

---

_Referenced in [clap-rs/clap#6190](../../clap-rs/clap/pulls/6190.md) on 2025-12-10 20:02_

---

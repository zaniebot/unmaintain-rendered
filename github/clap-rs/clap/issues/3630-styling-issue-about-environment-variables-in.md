---
number: 3630
title: Styling issue about environment variables in generated man pages
type: issue
state: closed
author: orhun
labels:
  - C-bug
  - E-easy
  - A-man
assignees: []
created_at: 2022-04-14T12:56:17Z
updated_at: 2022-04-20T13:50:13Z
url: https://github.com/clap-rs/clap/issues/3630
synced_at: 2026-01-07T13:12:19-06:00
---

# Styling issue about environment variables in generated man pages

---

_Issue opened by @orhun on 2022-04-14 12:56_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.59.0 (9d1b2106e 2022-02-23)

### Clap Version

master

### Minimal reproducible code

```rs
use clap::{arg, Command};
use clap_mangen::Man;
use std::io::{self, Result};

fn main() -> Result<()> {
    let cmd = Command::new("myapp").arg(
        arg!(-c --config <FILE> "Sets a custom config file")
            .long_help("Some more text about how to set a custom config file")
            .required(false)
            .takes_value(true)
            .default_value("config.toml")
            .env("CONFIG_FILE"),
    );

    Man::new(cmd).render(&mut io::stdout())
}
```

### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

There is a missing space between argument and environment variable description.

```roff
.TP
\fB\-c\fR, \fB\-\-config\fR=\fIFILE\fR [default: config.toml]
Some more text about how to set a custom config fileMay also be specified with the \fBCONFIG_FILE\fR environment variable.
                                                  ^^^ a space/newline is needed here
```


<details>
<summary>Full output</summary>

```roff
.ie \n(.g .ds Aq \(aq
.el .ds Aq '
.TH myapp 1  "myapp "
.SH NAME
myapp
.SH SYNOPSIS
\fBmyapp\fR [\fB\-h\fR|\fB\-\-help\fR] [\fB\-c\fR|\fB\-\-config\fR]
.SH DESCRIPTION
.SH OPTIONS
.TP
\fB\-h\fR, \fB\-\-help\fR
Print help information
.TP
\fB\-c\fR, \fB\-\-config\fR=\fIFILE\fR [default: config.toml]
Some more text about how to set a custom config fileMay also be specified with the \fBCONFIG_FILE\fR environment variable.
```

</details>


### Expected Behaviour

```roff
.TP
\fB\-c\fR, \fB\-\-config\fR=\fIFILE\fR [default: config.toml]
Some more text about how to set a custom config file. May also be specified with the \fBCONFIG_FILE\fR environment variable. 
                                                    ^^^
```

### Additional Context

Same issue exists in the [clap_mangen example](https://github.com/clap-rs/clap/blob/e8010e79a994dcc31b3c753969cbe2022eaeb0d8/clap_mangen/examples/man.rs).

### Debug Output

\-

---

_Label `C-bug` added by @orhun on 2022-04-14 12:56_

---

_Referenced in [clap-rs/clap#3631](../../clap-rs/clap/pulls/3631.md) on 2022-04-14 13:38_

---

_Label `E-easy` added by @epage on 2022-04-14 13:59_

---

_Label `A-man` added by @epage on 2022-04-14 13:59_

---

_Closed by @epage on 2022-04-20 13:50_

---

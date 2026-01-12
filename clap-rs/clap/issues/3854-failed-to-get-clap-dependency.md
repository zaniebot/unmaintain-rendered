```yaml
number: 3854
title: Failed to get clap dependency
type: issue
state: closed
author: iamgabrielsoft
labels:
  - C-bug
assignees: []
created_at: 2022-06-17T22:03:04Z
updated_at: 2022-06-18T22:37:15Z
url: https://github.com/clap-rs/clap/issues/3854
synced_at: 2026-01-12T16:14:15Z
```

# Failed to get clap dependency

---

_@iamgabrielsoft_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.61.0

### Clap Version

2.33.0

### Minimal reproducible code

```rust
    let cli = App::new("Editor")
        .version(VERSION)
        .author("Author: Gabriel <https://github.com/iamgabrielsoft>")
        .about("A Rust Editor")
        .arg(
            Arg::with_name("files")
                .multiple(true)
                .takes_value(true)
                .help(
                    r#"The files you wish to edit
                        You can also provide the line number to jump to by doing this:
                        file.txt:100 (This will go to line 100 in file.txt)"#,
                ),
        )
        .arg(
            Arg::with_name("readonly")
                .long("readonly")
                .short("r")
                .takes_value(false)
                .required(false)
                .help("Enable read only mode"),
        )
        .arg(
            Arg::with_name("config")
                .long("config")
                .short("c")
                .takes_value(true)
                // .default_value(&config_dir)
                .help("The directory of the config file"),
        );
```

### Steps to reproduce the bug with the above code

I ran the code multiple times and i still get the same error 

### Actual Behaviour

Caused by:
  failed to load source for dependency `clap`

Caused by:
  Unable to update registry `crates-io`

Caused by:
  failed to fetch `https://github.com/rust-lang/crates.io-index`

Caused by:
  failed to authenticate when downloading repository: git@github.com:rust-lang/crates.io-index

  * attempted ssh-agent authentication, but no usernames succeeded: `git`

  if the git CLI succeeds then `net.git-fetch-with-cli` may help here
  https://doc.rust-lang.org/cargo/reference/config.html#netgit-fetch-with-cli

Caused by:
  no authentication available

### Expected Behaviour

it should run smoothly without any error response from the compiler

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @iamgabrielsoft on 2022-06-17 22:03_

---

_Comment by @epage on 2022-06-18 00:15_

Looks like something weird is going on between cargo, your git configuration, and/or your SSH configuration.  I'd recommend reaching out to one of the user forums to get help debugging that issue.

---

_Closed by @epage on 2022-06-18 00:15_

---

_Comment by @iamgabrielsoft on 2022-06-18 22:37_

yeah i figured it out, its was coming from my ssh and github 

---

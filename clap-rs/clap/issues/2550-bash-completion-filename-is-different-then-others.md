```yaml
number: 2550
title: bash completion filename is different then others
type: issue
state: closed
author: XyLyXyRR
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2021-06-18T04:55:28Z
updated_at: 2021-07-21T23:31:07Z
url: https://github.com/clap-rs/clap/issues/2550
synced_at: 2026-01-12T16:14:13Z
```

# bash completion filename is different then others

---

_@XyLyXyRR_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.1 (9bc8c42bb 2021-05-09)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

`build.rs`
```rust
use clap::{Clap, IntoApp};
use clap_generate::{generate_to, generators::*};
use std::env;

#[derive(Clap)]
#[clap(bin_name=env!("CARGO_PKG_NAME"))]
struct Cli {}

fn main() {
    let mut app = Cli::into_app();
    let bin_name = &env::var("CARGO_PKG_NAME")
        .unwrap()
        .strip_suffix("-bin")
        .map(|x| x.to_string())
        .expect("strip '-bin' failed");
    let out_dir = "target/completions";
    std::fs::create_dir_all(out_dir).unwrap();

    generate_to::<Bash, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<Elvish, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<Fish, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<PowerShell, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<Zsh, _, _>(&mut app, bin_name, out_dir);
}
```


### Steps to reproduce the bug with the above code

- `cargo build`

### Actual Behaviour

`ls target/completions -1`:
```
_one
_one.ps1
one-bin.bash
one.elv
one.fish
```

### Expected Behaviour

`ls target/completions -1`:
```
_one
_one.ps1
one.bash
one.elv
one.fish
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @XyLyXyRR on 2021-06-18 04:55_

---

_Added to milestone `3.0` by @pksunkara on 2021-07-07 02:22_

---

_Label `C: generator` added by @pksunkara on 2021-07-07 02:22_

---

_Comment by @pksunkara on 2021-07-07 02:23_

Are there any conventions regarding these filenames? How does other tools name them?

---

_Comment by @XyLyXyRR on 2021-07-07 06:18_

I don't know.

---

_Comment by @epage on 2021-07-21 20:29_

I default to ripgrep as my guide since its had a lot of eyes.  Its behavior is what is expected in this Issue.

See https://github.com/BurntSushi/ripgrep/blob/master/.github/workflows/release.yml#L161

---

_Comment by @epage on 2021-07-21 20:37_

I tried to reproduce this but couldn't

`Cargo.toml`:
```toml
[package]
name = "foo-bin"
version = "0.1.0"
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = { path = "../clap" }
clap_generate = { path = "../clap/clap_generate" }
```

`src/main.rs`:
```rust
use clap::{Clap, IntoApp};
use clap_generate::{generate_to, generators::*};
use std::env;

#[derive(Clap)]
#[clap(bin_name=env!("CARGO_PKG_NAME"))]
struct Cli {}

fn main() {
    let mut app = Cli::into_app();
    let bin_name = &env::var("CARGO_PKG_NAME")
        .unwrap()
        .strip_suffix("-bin")
        .map(|x| x.to_string())
        .expect("strip '-bin' failed");
    let out_dir = "target/completions";
    std::fs::create_dir_all(out_dir).unwrap();

    generate_to::<Bash, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<Elvish, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<Fish, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<PowerShell, _, _>(&mut app, bin_name, out_dir.clone());
    generate_to::<Zsh, _, _>(&mut app, bin_name, out_dir);
}
```

Commands:
```bash
$ cargo run
$ ls target/completions/ -1
_foo
_foo.ps1
foo.bash
foo.elv
foo.fish
```

I also tried switching my `main.rs` to a `build.rs` and got the same

---

_Comment by @XyLyXyRR on 2021-07-21 23:21_

This is already fixed in master,  although I didn't find that fixed commit.

Use the version on crates.io will reproduce.

```Cargo.toml
[build-dependencies]
# clap = { path = "./clap" }
# clap_generate = { path = "./clap/clap_generate" }
clap = "3.0.0-beta.2"
clap_generate = "3.0.0-beta.2"
```

---

_Closed by @XyLyXyRR on 2021-07-21 23:31_

---

---
number: 996
title: "How does one pass a dynamic `String` to the `version` function?"
type: issue
state: closed
author: TomasTomecek
labels: []
assignees: []
created_at: 2017-07-07T09:26:27Z
updated_at: 2018-08-02T03:30:09Z
url: https://github.com/clap-rs/clap/issues/996
synced_at: 2026-01-10T01:26:40Z
---

# How does one pass a dynamic `String` to the `version` function?

---

_Issue opened by @TomasTomecek on 2017-07-07 09:26_

### Rust Version

rustc 1.18.0

### Affected Version of clap

```toml
[[package]]
name = "clap"
version = "2.20.5"
source = "registry+https://github.com/rust-lang/crates.io-index"
dependencies = [
 "bitflags 0.7.0 (registry+https://github.com/rust-lang/crates.io-index)",
 "unicode-segmentation 1.1.0 (registry+https://github.com/rust-lang/crates.io-index)",
 "unicode-width 0.1.4 (registry+https://github.com/rust-lang/crates.io-index)",
 "vec_map 0.6.0 (registry+https://github.com/rust-lang/crates.io-index)",
]
```

### Actual Behavior Summary
I am getting a compilation failure due to lifetimes mismatch. I am trying to
add commit hash from an environment variable and pass it to `version` function
of `App`.

### Sample Code or Link to Sample Code
```rust
use clap::{App,Arg,SubCommand};

fn get_version_str() -> String {
    let version: Option<&'static str> = option_env!("CARGO_PKG_VERSION");
    let commit: Option<&'static str> = option_env!("TRAVIS_COMMIT");
    let mut result: String;
    match version {
        Some(v) => result = String::from(v),
        None => result = String::from("<version undefined>"),
    };
    if let Some(v) = commit {
        result += &format!(" ({})", &v);
    }
    result
}


pub fn cli<'a, 'b>() -> App<'a, 'b> {
    let version = get_version_str();
    let version_ref: &'b str = version.as_str();
    App::new("pretty-git-prompt")
        .version(version_ref)
```

### Output

```rust
error[E0495]: cannot infer an appropriate lifetime due to conflicting requirements
  --> src/cli.rs:26:18
   |
26 |         .version(version_ref)
   |                  ^^^^^^^^^^^
   |
note: first, the lifetime cannot outlive the lifetime 'b as defined on the body at 20:36...
  --> src/cli.rs:20:37
   |
20 |   pub fn cli<'a, 'b>() -> App<'a, 'b> {
   |  _____________________________________^
21 | |     // FIXME: populate about with this
22 | |     // let ref def_conf_desc = format!("Create default config at \"{}\".", get_default_config_path().to_str().unwrap());
23 | |     let version = get_version_str();
...  |
39 | |              .help("Print debug messages, useful for identifying issues."))
40 | | }
   | |_^
note: ...so that expression is assignable (expected &str, found &'b str)
  --> src/cli.rs:26:18
   |
26 |         .version(version_ref)
   |                  ^^^^^^^^^^^
note: but, the lifetime must be valid for the lifetime 'a as defined on the body at 20:36...
  --> src/cli.rs:20:37
   |
20 |   pub fn cli<'a, 'b>() -> App<'a, 'b> {
   |  _____________________________________^
21 | |     // FIXME: populate about with this
22 | |     // let ref def_conf_desc = format!("Create default config at \"{}\".", get_default_config_path().to_str().unwrap());
23 | |     let version = get_version_str();
...  |
39 | |              .help("Print debug messages, useful for identifying issues."))
40 | | }
   | |_^
note: ...so that expression is assignable (expected clap::App<'a, 'b>, found clap::App<'_, '_>)
  --> src/cli.rs:25:5
   |
25 | /     App::new("pretty-zsh-prompt")
26 | |         .version(version_ref)
27 | |         .author("Tomas Tomecek <tomas@tomecek.net>")
28 | |         .about("Get `git status` inside your shell prompt.")
...  |
38 | |              .long("debug")
39 | |              .help("Print debug messages, useful for identifying issues."))
   | |___________________________________________________________________________^

error: aborting due to previous error

error: Could not compile `pretty-git-prompt`.
```

Can I get some help here please?

---

_Comment by @TomasTomecek on 2017-07-16 17:20_

I managed to figure this out by not meddling with lifetimes and have one single function which contains `App` instance and it's not passed anywhere else.

---

_Closed by @TomasTomecek on 2017-07-16 17:20_

---

---
number: 2425
title: Unable to validate YAML config
type: issue
state: closed
author: HitomiTenshi
labels:
  - C-enhancement
assignees: []
created_at: 2021-03-28T00:28:29Z
updated_at: 2021-06-01T21:44:52Z
url: https://github.com/clap-rs/clap/issues/2425
synced_at: 2026-01-07T13:12:19-06:00
---

# Unable to validate YAML config

---

_Issue opened by @HitomiTenshi on 2021-03-28 00:28_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.51.0 (2fd73fabe 2021-03-23)

### Clap Version

master @ 52814b893c87e1c0350cae13fc1988fe2aa9886a

### Minimal reproducible code

src/main.rs

```rust
use clap::{App, load_yaml};

fn main() {
    let _matches = App::from(load_yaml!("cli.yaml")).get_matches();
}
```

src/cli.yaml

```yaml
name: Test
version: "0.0.1"
args:
    - port:
          short: p
          long: port
          value_name: NUMBER
          default_value: "invalid text"
          regex_validator:
              - ^(?:6553[0-5]|655[0-2]\d|65[0-4]\d{2}|6[0-4]\d{3}|[1-5]\d{4}|[1-9]\d{0,3})$
              - Value needs to be a number between 1 and 65535
          about: |-
              Sets the port number to listen on
```

### Steps to reproduce the bug with the above code

cargo run
cargo run -- --port asd

### Actual Behaviour

Runs with no errors.

### Expected Behaviour

Panic:

```
thread 'main' panicked at 'Unknown Arg setting 'regex_validator' in YAML file for arg 'port''
```

### Additional Context

```toml
[dependencies]
clap = { git = "https://github.com/clap-rs/clap", rev = "52814b893c87e1c0350cae13fc1988fe2aa9886a", default-features = false, features = ["regex", "std", "yaml"] }
```

PR #2415 causes a regression. It takes away the ability to check if my `cli.yaml` contains correct parameters.

I accidentally wrote `regex_validator` instead of the correct term `validator_regex` and lost hours of trying to get this feature to work. I thought I had some problems with my regex which caused my turmoil.

To fix this issue I would recommend adding a top parameter e. g. `validate: true`, so one could still enjoy their YAML being validated for wrong parameters.

Either that or revert PR #2415. I don't see why having additional parameters in your YAML file, that is being used primarily for `clap`, has any use beyond adding comments, which you can already do by writing this:

```yaml
version: "0.0.1" # This is a comment in YAML
# This is a comment in a separate line
```

---

_Label `T: bug` added by @HitomiTenshi on 2021-03-28 00:28_

---

_Added to milestone `3.0` by @pksunkara on 2021-04-03 21:13_

---

_Label `T: bug` removed by @pksunkara on 2021-04-03 21:14_

---

_Label `C: yaml parser` added by @pksunkara on 2021-04-03 21:14_

---

_Label `T: enhancement` added by @pksunkara on 2021-04-03 21:14_

---

_Comment by @pksunkara on 2021-04-03 21:14_

@HitomiTenshi I would welcome a PR.

---

_Closed by @pksunkara on 2021-06-01 21:44_

---

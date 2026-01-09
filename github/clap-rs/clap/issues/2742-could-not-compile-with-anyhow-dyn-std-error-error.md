---
number: 2742
title: "Could not compile with `anyhow`: `(dyn std::error::Error + 'static)` cannot be sent between threads safely"
type: issue
state: closed
author: JakkuSakura
labels:
  - C-bug
assignees: []
created_at: 2021-08-29T08:00:27Z
updated_at: 2021-08-29T10:29:20Z
url: https://github.com/clap-rs/clap/issues/2742
synced_at: 2026-01-07T13:12:19-06:00
---

# Could not compile with `anyhow`: `(dyn std::error::Error + 'static)` cannot be sent between threads safely

---

_Issue opened by @JakkuSakura on 2021-08-29 08:00_


[project.zip](https://github.com/clap-rs/clap/files/7072114/project.zip)
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version
From
rustc 1.54.0-nightly (1c6868aa2 2021-05-27)
To
rustc 1.56.0-nightly (5eacec9ec 2021-08-28)

Can't compile

### Clap Version

3.0.0-beta.4 Can't compile

3.0.0-beta.2 Compiles

### Minimal reproducible code

```rust
fn foo() -> anyhow::Result<()> {
    let app = clap::App::new("foo");
    let matches = app.get_matches();
    matches.value_of_t::<String>("test")?;
    Ok(())
}
```


### Steps to reproduce the bug with the above code

cargo build

### Actual Behaviour

does not compile
```
fn foo() -> anyhow::Result<()> {
    let app = clap::App::new("foo");
    let matches = app.get_matches();
    matches.value_of_t::<String>("test")?;
    Ok(())
}
```

### Expected Behaviour

Compiles without error, like with earlier versions of rustc. I'm not sure if new rustc contains a breaking change, or a bug.

### Additional Context

https://github.com/dtolnay/anyhow/issues/164

### Debug Output

_No response_

---

_Label `T: bug` added by @JakkuSakura on 2021-08-29 08:00_

---

_Referenced in [rust-lang/rust#88455](../../rust-lang/rust/issues/88455.md) on 2021-08-29 08:06_

---

_Renamed from "Could not compile with `anyhow` on latest nightly rustc: `(dyn std::error::Error + 'static)` cannot be sent between threads safely" to "Could not compile with `anyhow`: `(dyn std::error::Error + 'static)` cannot be sent between threads safely" by @JakkuSakura on 2021-08-29 08:32_

---

_Referenced in [clap-rs/clap#2743](../../clap-rs/clap/pulls/2743.md) on 2021-08-29 08:41_

---

_Closed by @pksunkara on 2021-08-29 10:29_

---

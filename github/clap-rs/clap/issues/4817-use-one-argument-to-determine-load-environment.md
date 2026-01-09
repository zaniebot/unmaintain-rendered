---
number: 4817
title: Use one argument to determine load environment, before setting defaults
type: issue
state: closed
author: tgross35
labels:
  - C-enhancement
assignees: []
created_at: 2023-03-31T23:02:31Z
updated_at: 2023-03-31T23:17:30Z
url: https://github.com/clap-rs/clap/issues/4817
synced_at: 2026-01-07T13:12:20-06:00
---

# Use one argument to determine load environment, before setting defaults

---

_Issue opened by @tgross35 on 2023-03-31 23:02_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.13

### Describe your use case

I think there is a fairly common use case for environment variable defaults where you want to load a specific `.env` file based on a command argument. This means that the process would be: parse single argument, error or default if missing -> read .env based on parsed value -> parse rest of arguments -> do env defaults for those arguments -> do hardcoded defaults for those arguments -> error for missing values.

I don't know a good way to do this with Clap. Example use case:

```rust
#[derive(Parser, Debug)]
pub struct Config {
    #[clap(long, env = "MYAPP_ENV", default_value = "production")]
    pub mode: Mode
    #[clap(long, env = "MYAPP_PORT", default_value = "1234")]
    pub port: u16,
    #[clap(long, env = "MYAPP_BIND_ADDR", default_value = "0.0.0.0")]
    pub bind_addr: String,
}

#[derive(Clone, Copy, Debug, ValueEnum)]
pub enum Mode {
    Production,
    Development,
    Test
}
```


### Describe the solution you'd like

Some way to parse a single field of the struct - I'm imagining something like a `parse_first` attribute:

```rust
struct Config {
    #[clap(long, env = "MYAPP_ENV", default_value = "production", parse_first = true)]
    pub mode: Mode
    // ...
}
```

Which could create

```rust
// function to parse only the first values
impl Config { pub fn parse_first() -> ConfigParseFirst }
// dummy struct that holds values marked `parse_first`
struct ConfigParseFirst { pub mode: Mode } 
// continue parsing the rest of the values, then go to their defaults
// move `mode` to `Config` and don't reparse to avoid race condition type thingies
impl ConfigParseFirst { pub fn parse_rest(self) -> Config }
```

And then be used like:

```rust
fn main() {
    load_env(".env");
    let tmp_cfg = Config::parse_first();
    match tmp_cfg.mode {
        Production => load_env(".env.production"),
        Development => load_env(".env.development"),
        Test => load_env(".env.test")
    }
    let cfg = tmp_cfg.parse_remaining();
    todo!()
}
```

### Alternatives, if applicable

A callback method could also work, and maybe be simpler. E.g. 

```rust
struct Config {
    #[clap(long, env = "MYAPP_ENV", default_value = "production", parse_first_callback = set_the_mode)]
    pub mode: Mode
    // ...
}

fn set_the_mode(mode: &Mode) {
    match mode { /* ... */ }
}
```

### Additional Context

_No response_

---

_Label `C-enhancement` added by @tgross35 on 2023-03-31 23:02_

---

_Comment by @epage on 2023-03-31 23:14_

I believe you are looking for a mixture of either
- #2763
- #4793

I'm closing in favor of those.

---

_Closed by @epage on 2023-03-31 23:14_

---

_Comment by @tgross35 on 2023-03-31 23:17_

Good to know those exist, I had no clue what to look for. Thanks!

---

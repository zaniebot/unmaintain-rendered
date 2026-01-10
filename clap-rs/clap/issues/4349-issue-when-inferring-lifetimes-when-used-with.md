---
number: 4349
title: Issue when inferring lifetimes when used with serde.
type: issue
state: open
author: 0xForerunner
labels:
  - C-bug
  - E-hard
  - E-help-wanted
  - A-derive
assignees: []
created_at: 2022-10-05T08:02:30Z
updated_at: 2022-10-05T16:25:28Z
url: https://github.com/clap-rs/clap/issues/4349
synced_at: 2026-01-10T01:27:53Z
---

# Issue when inferring lifetimes when used with serde.

---

_Issue opened by @0xForerunner on 2022-10-05 08:02_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0-nightly (01af5040f 2022-10-04)

### Clap Version

clap = { version = "4.0.8", features = ["derive"] }

### Minimal reproducible code

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema, Subcommand)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    GetPrice {
        #[arg(value_parser = serde_json::from_str::<AssetInfo>)]
        asset:  AssetInfo,
    },
    GetPrices {
        #[arg(value_parser = serde_json::from_str::<Vec<AssetInfo>>)]
        assets: Vec<AssetInfo>,
    },
    GetAllPrices {
        #[arg(value_parser = serde_json::from_str::<Option<AssetInfo>>)]
        start_after:    Option<AssetInfo>,
        limit:          Option<u32>,
    },
}

#[derive(Serialize, Deserialize, Clone, Debug, Hash, PartialEq, Eq, JsonSchema, PartialOrd, Ord)]
#[serde(rename_all = "snake_case")]
#[repr(u8)]
pub enum AssetInfo {
    Token { contract_addr: Addr },
    NativeToken { denom: String },
}
```
```toml
clap = { version = "4.0.8", features = ["derive"] }
serde_json = "1.0"
serde = { version = "1.0.137", default-features = false, features = ["derive"] }
```

### Steps to reproduce the bug with the above code

This won't compile. 

### Actual Behaviour

```
error: implementation of `FnOnce` is not general enough
  --> packages/lending/src/price_oracle.rs:42:71
   |
42 | #[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema, Subcommand)]
   |                                                                       ^^^^^^^^^^ implementation of `FnOnce` is not general enough
   |
   = note: `fn(&'2 str) -> Result<AssetInfo, serde_json::Error> {serde_json::from_str::<'2, AssetInfo>}` must implement `FnOnce<(&'1 str,)>`, for any lifetime `'1`...
   = note: ...but it actually implements `FnOnce<(&'2 str,)>`, for some specific lifetime `'2`
   = note: this error originates in the derive macro `Subcommand` (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0308]: mismatched types
   --> packages/lending/src/price_oracle.rs:42:71
    |
42  | #[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema, Subcommand)]
    |                                                                       ^^^^^^^^^^ one type is more general than the other
    |
    = note: expected trait `for<'a> Fn<(&'a str,)>`
               found trait `Fn<(&str,)>`
note: the lifetime requirement is introduced here
   --> /Users/ewoolsey/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.8/src/builder/arg.rs:943:48
    |
943 |     pub fn value_parser(mut self, parser: impl IntoResettable<super::ValueParser>) -> Self {
    |                                                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    = note: this error originates in the derive macro `Subcommand` (in Nightly builds, run with -Z macro-backtrace for more info)

error: implementation of `FnOnce` is not general enough
  --> packages/lending/src/price_oracle.rs:42:71
   |
42 | #[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema, Subcommand)]
   |                                                                       ^^^^^^^^^^ implementation of `FnOnce` is not general enough
   |
   = note: `fn(&'2 str) -> Result<std::vec::Vec<AssetInfo>, serde_json::Error> {serde_json::from_str::<'2, std::vec::Vec<AssetInfo>>}` must implement `FnOnce<(&'1 str,)>`, for any lifetime `'1`...
   = note: ...but it actually implements `FnOnce<(&'2 str,)>`, for some specific lifetime `'2`
   = note: this error originates in the derive macro `Subcommand` (in Nightly builds, run with -Z macro-backtrace for more info)

error: implementation of `FnOnce` is not general enough
  --> packages/lending/src/price_oracle.rs:42:71
   |
42 | #[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema, Subcommand)]
   |                                                                       ^^^^^^^^^^ implementation of `FnOnce` is not general enough
   |
   = note: `fn(&'2 str) -> Result<std::option::Option<AssetInfo>, serde_json::Error> {serde_json::from_str::<'2, std::option::Option<AssetInfo>>}` must implement `FnOnce<(&'1 str,)>`, for any lifetime `'1`...
   = note: ...but it actually implements `FnOnce<(&'2 str,)>`, for some specific lifetime `'2`
   = note: this error originates in the derive macro `Subcommand` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0308`.
warning: `lending` (lib) generated 1 warning
error: could not compile `lending` due to 4 previous errors; 1 warning emitted
```

### Expected Behaviour

This is significantly above my head. Not sure what the solution is.

### Additional Context

_No response_

### Debug Output

nothing changed.

---

_Label `C-bug` added by @0xForerunner on 2022-10-05 08:02_

---

_Comment by @epage on 2022-10-05 13:21_

1. Could you provide a complete reproduction case?
2. Is that the full list of errors?  We've sometimes seen a different error was actually the cause of that specific error
3. Are all of your field types `Clone`?

---

_Comment by @0xForerunner on 2022-10-05 15:41_

1. right I forgot Addr.
```
#[derive(
    Serialize, Deserialize, Clone, Debug, PartialEq, Eq, PartialOrd, Ord, Hash, JsonSchema,
)]
pub struct Addr(String);
```
2. yes this was the full list of errors.
3. Yes they are.

---

_Comment by @epage on 2022-10-05 16:04_

Please next time, include a full reproduction case so I don't have to work backwards to create all of the scaffolding that was elided.

Below is what I"m using for this issue.  It goes a step further and uses `rust-script` to include the manifest.
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "4.0.8", features = ["derive", "wrap_help", "env"] }
//! serde_json = "1.0"
//! serde = { version = "1.0.137", features = ["derive"] }
//! ```

use clap::Parser;
use clap::Subcommand;
use serde::Deserialize;
use serde::Serialize;

#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    #[command(subcommand)]
    command: QueryMsg,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Subcommand)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    GetPrice {
        #[arg(value_parser = serde_json::from_str::<AssetInfo>)]
        asset: AssetInfo,
    },
    GetPrices {
        #[arg(value_parser = serde_json::from_str::<Vec<AssetInfo>>)]
        assets: Vec<AssetInfo>,
    },
    GetAllPrices {
        #[arg(value_parser = serde_json::from_str::<Option<AssetInfo>>)]
        start_after: Option<AssetInfo>,
        limit: Option<u32>,
    },
}

#[derive(Serialize, Deserialize, Clone, Debug, Hash, PartialEq, Eq, PartialOrd, Ord)]
#[serde(rename_all = "snake_case")]
#[repr(u8)]
pub enum AssetInfo {
    Token { contract_addr: Addr },
    NativeToken { denom: String },
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Eq, PartialOrd, Ord, Hash)]
pub struct Addr(String);

fn main() {
    let args: Cli = Cli::parse();
    println!("{:?}", args);
}
```

---

_Comment by @epage on 2022-10-05 16:08_

Still double checking some things but the following works around the problem:
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "4.0.8", features = ["derive", "wrap_help", "env"] }
//! serde_json = "1.0"
//! serde = { version = "1.0.137", features = ["derive"] }
//! ```

use clap::Parser;
use clap::Subcommand;
use serde::Deserialize;
use serde::Serialize;

#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    #[command(subcommand)]
    command: QueryMsg,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Subcommand)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    GetPrice {
        #[arg(value_parser = asset_parser)]
        asset: AssetInfo,
    },
    GetPrices {
        #[arg(value_parser = asset_parser)]
        assets: Vec<AssetInfo>,
    },
    GetAllPrices {
        #[arg(value_parser = asset_parser)]
        start_after: Option<AssetInfo>,
        limit: Option<u32>,
    },
}

fn asset_parser(s: &str) -> Result<AssetInfo, serde_json::Error> {
    serde_json::from_str::<AssetInfo>(s)
}

#[derive(Serialize, Deserialize, Clone, Debug, Hash, PartialEq, Eq, PartialOrd, Ord)]
#[serde(rename_all = "snake_case")]
#[repr(u8)]
pub enum AssetInfo {
    Token { contract_addr: Addr },
    NativeToken { denom: String },
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Eq, PartialOrd, Ord, Hash)]
pub struct Addr(String);

fn main() {
    let args: Cli = Cli::parse();
    println!("{:?}", args);
}
```

Besides breaking out `asset_parser`, this fixes the `value_parser` so it generates the actual type (`AssetInfo`).  `Vec` and `Option` are handled by clap as part of the parsing arguments.

---

_Comment by @epage on 2022-10-05 16:13_

I think this error is key:
```
error[E0308]: mismatched types
   --> clap-4349.rs:22:59
    |
22  | #[derive(Serialize, Deserialize, Clone, Debug, PartialEq, Subcommand)]
    |                                                           ^^^^^^^^^^ one type is more general than the other
    |
    = note: expected trait `for<'r> Fn<(&'r str,)>`
               found trait `Fn<(&str,)>`
```

`from_str` is defined as:
```rust
pub fn from_str<'a, T>(s: &'a str) -> Result<T> where
    T: Deserialize<'a>,
```
and I suspect the `T: Deserialize<'a>` bound is confusing things.

I am not aware of a way to loosen things so we can work with `serde_json::from_str` without something clarifying the types to the compiler, like an intermediate function.

---

_Label `E-hard` added by @epage on 2022-10-05 16:13_

---

_Label `A-derive` added by @epage on 2022-10-05 16:13_

---

_Label `E-help-wanted` added by @epage on 2022-10-05 16:13_

---

_Comment by @0xForerunner on 2022-10-05 16:25_

Thanks for your hard work! Really appreciate it. If you have any ideas, let me know!

---

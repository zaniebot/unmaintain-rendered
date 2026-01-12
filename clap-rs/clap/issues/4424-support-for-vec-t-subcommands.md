```yaml
number: 4424
title: "Support for Vec<T> Subcommands"
type: issue
state: closed
author: 0xForerunner
labels:
  - C-enhancement
assignees: []
created_at: 2022-10-26T01:41:22Z
updated_at: 2022-10-26T15:03:46Z
url: https://github.com/clap-rs/clap/issues/4424
synced_at: 2026-01-12T16:14:16Z
```

# Support for Vec<T> Subcommands

---

_@0xForerunner_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.18

### Describe your use case

I would like to parse a vector of subcommands like so:

```rust
#[derive(Parser)]
#[clap(rename_all = "snake_case")]
pub enum MyCli {
    GetPrices {
        #[command(subcommand)]
        assets: Vec<AssetInfo>,
    },
}

#[derive(Subcommand)]
#[clap(rename_all = "snake_case")]
pub enum AssetInfo {

    #[command(subcommand)]
    Base(BaseExecuteMsg),

    SetConfig {
        #[command(flatten)]
        set_config_msg: SetConfigMsg,
    },

    AddAsset {
        #[command(flatten)]
        asset_info_type: AddAssetType,
    },

    RemoveAsset {
        #[command(subcommand)]
        asset_info: RemoveAssetType,
    },

    UpdatePrice {
        #[command(subcommand)]
        asset_info: PriceType,
    },

    UpdatePrices {
        #[arg(value_parser=tuple_parser::<PriceType, Decimal256>)]
        assets: Vec<(PriceType, Decimal256)>,
    },
}
```

### Describe the solution you'd like

I'm not sure what the best internal solution is here.

---

_Label `C-enhancement` added by @0xForerunner on 2022-10-26 01:41_

---

_Comment by @epage on 2022-10-26 02:00_

> I would like to parse a vector of subcommands like so:

What do you mean by this?  Could you provide a complete program (e.g. `AssetInfo` is missing) and example runs of the program to help clarify the point?

---

_Comment by @0xForerunner on 2022-10-26 02:41_

@epage The above is sort of a toy example as my actual use case has dozens of nested subcommands. I realize the workaround is probably to use a value parser, but that doesn't work for me since 1: its insanely clunky, 2:  I'm also parsing and that would break the interactivity. For my specific use case I cannot change the structure of the enum since it is part of an external API.

---

_Comment by @epage on 2022-10-26 11:21_

Could you provide example runs of the program to clarify what the semantics of this feature would be?

---

_Comment by @0xForerunner on 2022-10-26 14:48_

Sure, as an example lets create something simpler and complete.

```rust
#[derive(Parser)]
#[clap(rename_all = "snake_case")]
pub enum MyCli {
    GetPrices {
        #[command(subcommand)]
        assets: Vec<AssetInfo>,
    },
}

#[derive(Subcommand)]
#[clap(rename_all = "snake_case")]
pub enum AssetInfo {
    Token {
        /// <string>, "atom3h6lk23h6"
        contract_addr: String
    },
    NativeToken {
        /// <string>, "uatom"
        denom: String
    },
}
```
This is a much simpler case then I have in reality, but useful for examining how this could work. An example run of the program could look like:
```bash
my-cli get_prices token atom3h6lk23h6 native_token uatom 
```
Ideally this should be implemented for any type that is IntoIterator<Item = T>. When these example args are parsed it would result in: 
```rust
MyCli::GetPrices{
    assets: vec![
        AssetInfo::Token { contract_addr: atom3h6lk23h6 },
        AssetInfo::NativeToken { denom: uatom }
    ]
}

---

_Comment by @epage on 2022-10-26 14:51_

Sounds like this is a duplicate of #2222.  Do I have that correct?

---

_Comment by @0xForerunner on 2022-10-26 14:57_

> Sounds like this is a duplicate of #2222. Do I have that correct?

I think depending on how it is implemented it could possibly be a duplicate. You mention in that thread disabling subcommands though, which would sort of defeat the purpose of what I'm going for no?

---

_Comment by @epage on 2022-10-26 15:03_

I'd prefer issues to focus on the requirements and not implementation.  As both of these are about command chaining, let's continue the conversation over there.  If you need sub-subcommands for your chained commands, I'd recommend posting in there with the reasons why its needed and a proposal for what the semantics would be (e.g. do we stop chaining when entering a sub-sub-command or do we back out to the parent chained command?)

---

_Closed by @epage on 2022-10-26 15:03_

---

_Referenced in [clap-rs/clap#2222](../../clap-rs/clap/issues/2222.md) on 2023-03-27 18:07_

---

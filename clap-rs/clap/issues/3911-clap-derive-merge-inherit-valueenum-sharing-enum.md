```yaml
number: 3911
title: "clap_derive: Merge/inherit `ValueEnum` (sharing enum variants in `ValueEnum`)"
type: issue
state: open
author: KSXGitHub
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-07-12T14:28:08Z
updated_at: 2022-07-12T14:45:08Z
url: https://github.com/clap-rs/clap/issues/3911
synced_at: 2026-01-12T16:14:15Z
```

# clap_derive: Merge/inherit `ValueEnum` (sharing enum variants in `ValueEnum`)

---

_@KSXGitHub_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.18

### Describe your use case

I have multiple flags that use various `value_enum`. These `value_enum`s share some of their variants.

### Describe the solution you'd like

Allow including one `value_enum` in the other. Like so:

```rust
#[derive(Clone, ValueEnum)]
enum Serde {
    Json,
    Yaml,
    Toml,
}

#[derive(Clone, ValueEnum)]
enum SerdePlain {
    Plain,
    #[clap(flatten)]
    Serde(Serde),
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @KSXGitHub on 2022-07-12 14:28_

---

_Label `A-derive` added by @epage on 2022-07-12 14:39_

---

_Label `S-waiting-on-design` added by @epage on 2022-07-12 14:39_

---

_Comment by @epage on 2022-07-12 14:45_

The main limitation is the current trait design
```rust
pub trait ValueEnum: Sized + Clone {
    fn value_variants<'a>() -> &'a [Self];
    fn from_str(input: &str, ignore_case: bool) -> Result<Self, String> {
        ...
    }
    fn to_possible_value<'a>(&self) -> Option<PossibleValue<'a>>;
}
```

`value_variants` requires a fixed size at the moment.

#2799 discussed a different part of the API but had a similar aspect of "should we allocate as part of `value_variants`?".

If we could support `impl Trait` in return position in traits, then we could have `value_variants` return that and chain the flatten calls together.

---

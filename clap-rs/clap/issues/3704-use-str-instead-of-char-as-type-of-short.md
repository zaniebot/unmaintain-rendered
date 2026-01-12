```yaml
number: 3704
title: "use &str instead of char as type of `short`"
type: issue
state: closed
author: franklucky001
labels:
  - C-enhancement
assignees: []
created_at: 2022-05-08T06:46:16Z
updated_at: 2022-05-09T14:30:21Z
url: https://github.com/clap-rs/clap/issues/3704
synced_at: 2026-01-12T16:14:15Z
```

# use &str instead of char as type of `short`

---

_@franklucky001_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.0

### Describe your use case

Because many professional nouns have fixed abbreviations, it is unfriendly to use only the initial letter as short name and is prone to conflict. Therefore, using `&str` as the short parameter type may be a better choice.

```rust
#[derive(Debug, Args)]
struct TrainingArgs{
    #[clap(short, long, default_value_t=1e-3)]
    learning_rate: f64,
    #[clap(short, long, default_value_t=10)]
    n_epochs: u32,
    #[clap(short, long, default_value_t=0.5)]
    dropout: f32,
    #[clap(short, long, default_value_t=128)]
    batch_size: u32,
    #[clap(short, long, default_value_t=1500)]
    max_iter_no_improve: u32,
    #[clap(short, long, default_value_t=100)]
    n_iter_to_eval: u32
}

```
- learning-rate use short name `lr` instead of `l`
- short name `n` of `n_epochs` conflicts with that of  `n_iter_to_eval`

### Describe the solution you'd like

```rust
#[derive(Debug, Args)]
struct TrainingArgs{
    #[clap(short="lr", long, default_value_t=1e-3)]
    learning_rate: f64,
    #[clap(short, long, default_value_t=10)]
    n_epochs: u32,
    #[clap(short="N", long, default_value_t=0.5)]
    dropout: f32,
    #[clap(short, long, default_value_t=128)]
    batch_size: u32,
    #[clap(short, long, default_value_t=1500)]
    max_iter_no_improve: u32,
    #[clap(short="n_iter", long, default_value_t=100)]
    n_iter_to_eval: u32
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @franklucky001 on 2022-05-08 06:46_

---

_Comment by @epage on 2022-05-09 14:30_

Shorts are not just an abbreviated aliases for longs, there is a long history of them having their own conventions that would be ambiguous with multi-letter shorts.
- Shorts can be concatenated together, like `foo -a -b -c` can be `foo -abc`
- Shorts can have their value concatenated, like `foo -a value` can be `foo -avalue`

What you are wanting for the type of abbreviations you mention are [Arg::visible_alias](https://docs.rs/clap/latest/clap/struct.Arg.html#method.visible_alias)

As this runs counter to our intentional design principles, I'm going to close this.

---

_Closed by @epage on 2022-05-09 14:30_

---

```yaml
number: 4900
title: "complete: Add `Shell` iterator"
type: issue
state: closed
author: CosmicHorrorDev
labels:
  - C-enhancement
assignees: []
created_at: 2023-05-12T04:22:25Z
updated_at: 2023-05-13T18:36:46Z
url: https://github.com/clap-rs/clap/issues/4900
synced_at: 2026-01-12T16:14:16Z
```

# complete: Add `Shell` iterator

---

_@CosmicHorrorDev_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.7

### Describe your use case

It would be nice to have an iterator over all `Shell`'s for generating all possible completions. It would allow for something like

```rust
for shell in Shell::iter() {
    let cmd = Opts::command();
    generate_to(shell, &mut cmd, "bin", &out_dir).unwrap();
}
```

### Describe the solution you'd like

Have an `::iter()` method on `Shell` that returns an iterator over all shell variants

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @CosmicHorrorDev on 2023-05-12 04:22_

---

_Referenced in [clap-rs/clap#4901](../../clap-rs/clap/pulls/4901.md) on 2023-05-12 04:41_

---

_Comment by @CosmicHorrorDev on 2023-05-12 04:42_

#4901 demonstrates how this could be implemented, but I'm open to design discussions and other considerations

---

_Comment by @epage on 2023-05-12 08:03_

Do you need this since it implements `ValueEnum::value_variants`

---

_Comment by @CosmicHorrorDev on 2023-05-12 19:38_

I somehow totally missed that when looking over things. Yes that covers my use case!

Would you be open to me adding a new example or extending the existing one for `generate_to()`? It seems to be an appropriate place since the common case would be generating all possible completion files

---

_Comment by @epage on 2023-05-13 05:26_

> Would you be open to me adding a new example or extending the existing one for generate_to()? It seems to be an appropriate place since the common case would be generating all possible completion files

Sure

---

_Referenced in [clap-rs/clap#4903](../../clap-rs/clap/pulls/4903.md) on 2023-05-13 17:21_

---

_Closed by @epage on 2023-05-13 18:36_

---

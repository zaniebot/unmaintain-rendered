---
number: 3930
title: Switch native completions to encourage env var initialization
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-docs
  - A-completion
  - E-easy
assignees: []
created_at: 2022-07-13T22:37:43Z
updated_at: 2024-08-12T16:54:34Z
url: https://github.com/clap-rs/clap/issues/3930
synced_at: 2026-01-07T13:12:20-06:00
---

# Switch native completions to encourage env var initialization

---

_Issue opened by @epage on 2022-07-13 22:37_

Maintainer's notes

We should document each solution (flag, subcommand, env var) and the trade offs.  We should probably make env variable the default.

---

### Discussed in https://github.com/clap-rs/clap/discussions/3926

<div type='discussions-op-text'>

<sup>Originally posted by **AndreasBackx** July 13, 2022</sup>
We have a nice macro that automatically adds autocomplete to your Clap CLI. We are currently doing this via a subcommand `generate-completions`. Though we realise that this approach makes it impossible to automatically generate completions for certain CLIs:

```rust
#[derive(Parser, Debug)]
pub struct Args {
    foo: String,
}
```

Imagine we have that very simple CLI and we want to add a subcommand. `foo` is a required argument so if we want to automate the autocomplete generation by adding `eval $(cli generate-completions)` or by exporting it to `/usr/share/bash-completion/completions/cli` for Bash, the CLI will complain that `foo` wasn't passed.

Therefore it seems right now that we should avoid depending on Clap parsing as part of autocomplete generation. Perhaps the people at Click realised this earlier as that is what they are doing: https://click.palletsprojects.com/en/8.1.x/shell-completion/.

So I have a few questions that spring from this:
1. Is my understanding correct? Would love to hear there might be a mistake.
2. What do you think of the solution, can you perhaps think of another solution?
3. Should the documentations surrounding this perhaps be updated?
4. With this in mind, perhaps we could integrate clap_complete into clap as a feature? If enabled, you can get autocomplete automatically in your CLI in a similar way to Click.


_Funnily enough we didn't initially face this problem because of how `command_for_update` sets required to false, see #3924._</div>

---

_Label `C-enhancement` added by @epage on 2022-07-13 22:37_

---

_Label `A-docs` added by @epage on 2022-07-13 22:37_

---

_Label `A-completion` added by @epage on 2022-07-13 22:37_

---

_Label `E-easy` added by @epage on 2022-07-13 22:37_

---

_Comment by @epage on 2024-08-10 02:43_

We now offer a subcommand for rust-native completions.  Closing in favor of that.  Stabilizing this is tracked in #3166

---

_Closed by @epage on 2024-08-10 02:43_

---

_Reopened by @epage on 2024-08-10 12:02_

---

_Comment by @epage on 2024-08-10 12:07_

As we are no longer documenting but providing a solution, I'm going to repurpose this to change native completions to use the env variable approach
- Good for performance:; don't need to go through application initialization, `Command` initialization, etc
- Good for build times / implementation complexity: we don't need to use the derive or hand implement the derive traits

What we could do is offer both approaches, each behind a feature.

Steps
1. Re-organize `shells/` where all shells are in `shells.rs` and the trait, trait impls, complete args, complete commands are in a `cmd.rs`
2. Add a `impl<F: Fn() -> Command> CommandFactory for F`
3. Add an `env.rs` with a builder for env var handling that takes an env var and a `CommandFactory` type, and a `try_eval` and a `eval`
4. Put Each of the above behind a feature

---

_Renamed from "Document different options for activating completions" to "Switch native completions to encourage env var initialization" by @epage on 2024-08-10 12:07_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2024-08-10 12:07_

---

_Referenced in [clap-rs/clap#5671](../../clap-rs/clap/pulls/5671.md) on 2024-08-12 16:17_

---

_Closed by @epage on 2024-08-12 16:54_

---

---
number: 6117
title: Provide empty implementations for Args and Subcommand
type: issue
state: closed
author: alexkazik
labels:
  - C-enhancement
  - E-easy
  - A-derive
assignees: []
created_at: 2025-08-31T08:08:33Z
updated_at: 2025-09-02T17:00:41Z
url: https://github.com/clap-rs/clap/issues/6117
synced_at: 2026-01-10T01:28:22Z
---

# Provide empty implementations for Args and Subcommand

---

_Issue opened by @alexkazik on 2025-08-31 08:08_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

I have a kind of framework which is used in other crates but should be expandable:

```rust
// Simplified example
#[derive(Parser)]
#[command(version, about, long_about = None)]
pub struct MyArgs<A: Args, S: Subcommand> {
    #[command(flatten)]
    custom_args: A,
    #[command(subcommand)]
    command: MySubcommand<S>,
}

#[derive(Subcommand)]
enum MySubcommand<S: Subcommand> {
    Run,
    #[command(flatten)]
    CustomCommand(S),
}
```

In order to to simply opt out of the custom stuff I also export this:
```rust
#[derive(Args)]
pub struct NoCustomArgs;
#[derive(Subcommand)]
pub enum NoCustomCommand {}
```

But I'd prefer if clap would have such "empty" implementations.

### Describe the solution you'd like

I've created `impl Args for ()` and `impl Subcommand for Infallible`:

https://github.com/alexkazik/forked-clap/commit/c06a8f1c7a0ef5d2d41146f02edff47a55a71591

I'd prefer `!` (the never type) over Infallible but that is still nightly.

If that is something which is desired I can create a PR.


### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @alexkazik on 2025-08-31 08:08_

---

_Comment by @epage on 2025-09-02 14:08_

> impl Subcommand for Infallible:

Why this instead of `for ()`?

---

_Label `A-derive` added by @epage on 2025-09-02 14:08_

---

_Comment by @alexkazik on 2025-09-02 14:26_

> > impl Subcommand for Infallible:
> 
> Why this instead of `for ()`?

Since NoCustomCommand is like Infallible (and enum without variants) I used that. 

And this way the subcommand can never be created (unlike `()`). 

Since it means that you don't want to provide an subcommand I though it's better to not even be capable of doing so.

---

_Comment by @epage on 2025-09-02 14:31_

Thinking more on this, `impl Subcommand for ()` would be helpful with `#[command(subcommand)]` while `impl Subcommand for Infallable` would be helpful for `#[command(flatten)]`, so seems like we should have both.

I'm ok with moving forward with all 3 impls.

---

_Label `E-easy` added by @epage on 2025-09-02 14:31_

---

_Referenced in [clap-rs/clap#6119](../../clap-rs/clap/pulls/6119.md) on 2025-09-02 16:03_

---

_Closed by @epage on 2025-09-02 17:00_

---

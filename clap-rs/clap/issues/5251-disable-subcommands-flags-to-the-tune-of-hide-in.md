---
number: 5251
title: "`disable` subcommands / flags, to the tune of `hide` in `derive` mode"
type: issue
state: open
author: abesto
labels:
  - C-enhancement
assignees: []
created_at: 2023-12-07T11:10:40Z
updated_at: 2023-12-07T16:04:57Z
url: https://github.com/clap-rs/clap/issues/5251
synced_at: 2026-01-10T01:28:09Z
---

# `disable` subcommands / flags, to the tune of `hide` in `derive` mode

---

_Issue opened by @abesto on 2023-12-07 11:10_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.7

### Describe your use case

Imagine a CLI shipped to multiple environments (i.e. both development servers and laptops). Some features (i.e. subcommands) only make sense in one or the other environment. I'd like to *disable* (instead of hide) the irrelevant subcommands. The condition is not (cannot be) `const`.

### Describe the solution you'd like

Instead of 

```rs
#[derive(Subcommand)]
enum Command {
    ...
    #[clap(hide = is_laptop())]
    SomeSubcommand { ... }
}
```

I'd love to write

```rs
#[derive(Subcommand)]
enum Command {
    ...
    #[clap(disable = is_laptop())]
    SomeSubcommand { ... }
}
```


### Alternatives, if applicable

* Conditional compilation is not an option because the condition is not `const`. Even if it was, it'd mean we'd need conditional compilation around both the definition of the enum item, *and also* every place the enum value is used, which is not great ergonomically.
* We could maybe split out the conditional parts into a separate struct, maybe use Builder mode, and flatten that into the main subcommands enum? Again, not great ergonomics.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @abesto on 2023-12-07 11:10_

---

_Comment by @epage on 2023-12-07 15:49_

A workaround for this is to conditionally
- Hide the subcommand
- Error if the subcommand gets activated

Since its possible to do, is there a reason we need to build this in directly to clap?

We have to weigh
- Features
- API overload (API too large to find the feature you need, creating diminishing returns for each feature we add)
- Build time
- Binary size

This feels like a less common case such that a native solution might not be justified when balancing out these competing demands.

That said, I think the way I would do this is to instead focus on a `CommandAction` system like our `ArgAction` which we've wanted to generalize with user actions.  There is less built-in functionality to Commands so it has been a lower priority.  With user-provided actions, you could create and action that conditionally errors.  I think this would also be true if we made the validation plugin system we've talked about.

---

_Comment by @abesto on 2023-12-07 16:04_

> This feels like a less common case such that a native solution might not be justified when balancing out these competing demands.

Yep, I buy that.

> With user-provided actions, you could create and action that conditionally errors.

Yeah, that'd lead to neater code than raising failures where the subcommand happens to be handled.

I guess it's possible (now, explicitly; or later, in a custom action / validator) to raise a "no such command" error, such that this case would be mostly indistinguishable from a situation where the subcommand *actually* didn't exist?

---

_Referenced in [clap-rs/clap#5935](../../clap-rs/clap/issues/5935.md) on 2025-03-03 15:38_

---

_Referenced in [clap-rs/clap#6185](../../clap-rs/clap/issues/6185.md) on 2025-11-18 16:30_

---
